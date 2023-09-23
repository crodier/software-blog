# Zero-Copy vs. Binary vs. JSON: A Comparative Analysis of Efficient Message Formats

At Layer 4 (Transport) in the OSI model 
you first choose TCP or UDP
as one important choice for messaging; but this is almost
always TCP due to error correction (along with de minimus
gains of using UDP over TCP and added complexity cost.)

Layer 7 (Application) is a critical decision 
of message format.  For any reasonably large messaging
system this decision can determine millions of dollars
of CPU and engineering time wasted vs. a better approach.

The results and reasons seem to be poorly understood
and the choice these days is for the ubiquitous JSON.

This decision for JSON be costly and troublesome for many
businesses.

# Intro:  Binary vs. JSON

A JSON message for a list of Users may look like this:
```json
[
  {
    "first": "Chris",
    "Last": "Rodier",
    "Birthday": "1/1/1900"
  },
  {
    "first": "foo",
    "Last": "bar",
    "Birthday": "12/31/2022"
  }
]
```
Each character must be serialized. to put this on the wire.
In thi example, we are sending more characters for the
key names than the actual data when we serialized the JSON
above.  In most message cases this overhead is minor
when the speed is not important and you will not be
sending the records over the wire often.  While this
is the case for many businesses when they start, it
is **almost never** the case for a mature business that message
speed and size is unimportant.

We can note that the keys of the JSON are duplicated
and it will be almost twice as expensive to send along
with each set of user data.  What if we replace
them with ```a,b,c``` as the keys, and store 
in some kind of key dictionary config
file we could distribute?

```json
[
  {
    "a": "Chris",
    "b": "Rodier",
    "c": "1/1/1900"
  },
  {
    "a": "foo",
    "b": "bar",
    "c": "12/31/2022"
  }
]
```

This will be smaller.  But it would be major engineering
work and complexity to manage this format; and we 
would have invented something 
similar to [Message Pack](https://msgpack.org/)

## Message Pack
If we take the original JSON array and were to use
a library called Message Pack to serialize it; the wire
format is something like our `a,b,c` compression.
Message pack will put the keys first and then use
tokens for the keys to avoid copying the entire strings
for every message in a form of message compression.

For example, our message as Message Pack serialized:
```
\x92\x83\xa5first\xa5Chris\xa4Last\xa6Rodier\xa8Birthday\xb41/1/1900\x83\xa5first\xa3foo\xa4Last\xa3bar\xa8Birthday\xb412/31/2022
```
For this one problem, MessagePack is effective.  
The keys have been compressed
by sending only once for a list of the same messages.  

But for sending one message it is almost identical:
```
import msgpack
import json

# User data in JSON format
user_json = {
    "first": "Chris",
    "Last": "Rodier",
    "Birthday": "1/1/1900"
}

# Serialize User data to MessagePack
user_messagepack = msgpack.packb(user_json)

# Calculate byte sizes
json_size = len(json.dumps(user_json).encode('utf-8'))
messagepack_size = len(user_messagepack)

print(f"JSON Size: {json_size} bytes")
print(f"MessagePack Size: {messagepack_size} bytes")
```

### What have we saved with Message Pack? Not much!

This python code prints:
```
JSON Size: 60 bytes
MessagePack Size: 43 bytes
```
This experiment revealed we use **60** bytes with message pack
vs. **43** bytes for JSON.  

With a minor **29%** improvement on size and **17** bytes saved,
we have though used an additional library across our code; this
does not seem to be worth complexity cost.

Indeed, MessagePack is a case where *the
squeeze is not worth the juice.*  This squeeze is
the engineering cost to introduce MessagePack everywhere
in your system.

MessagePack also lacks other
critical and helpful messaging features which we will now explore.
MessagePack gives a good idea of the type of optimization
we want to achieve; at least for wire formats.

### GZIP Compression

Instead of using another
library across the code, we can enable GZIP compression
for messages going between services and even to a browser.
This can be turned on as a feature of a Java server
and the browser will interpret and decompress the GZIP
for you without even changing the JavaScript code.
This use of compression is a much easier win than
using MessagePack.

### Cost of GZIP = CPU
The cost of the GZIP compression is CPU cost.  
GZIP is incredible, or you could experiment with Snappy.
Both of these are efficient and the CPU cost is tiny
for a small number of messages and the CPU compresses
small messages almost instantly.  Sending smaller messages
is always faster.  A question exists and is unique
to your network and CPUs; is compressing and sending
fewer bytes faster or slower than sending the bytes
without compression?  Given the fast speed of networks
and if systems are in the cloud the network is generally
fast enough where it is a small win to compress but
not significant enough for small messaging cases.
The CPU speed at 4GHZ though makes it typically a win
these days of some small degree although in some cases
with certain CPUs it will be slower to compress and send
(think Rasberry Pi.)  The win on any machine
is small enough that it should be tested first and not 
assumed it will be faster compressed as it varies
for the messages and the compression used.  Furthermore,
when you compress you may be stealing valuable CPU from
your application.  While compression takes very small
CPU for small messages, for large messages and large
arrays of large messages it is measured in tens of 
seconds to compress and decompress the messages, and 
at a high CPU cost.  CPUs are not yet infinitely fast;
quantum may change this but until then there is a 
clear cost-benefit to using compression.

What if this cost could be eliminated.  What if we 
could pre-agree on the message keys in an elegant way?  

This is what binary message formats accomplish.

## GZIP Experiment
```python

import json
import gzip
import sys
import json

# Define the User JSON data
user_data = {
    "first": "Chris",
    "Last": "Rodier",
    "Birthday": "1/1/1900"
}

# Convert the JSON data to a JSON string
json_string = json.dumps(user_data)

# Compress the JSON string using GZIP
compressed_data = gzip.compress(json_string.encode("utf-8"))

# Calculate the original and compressed data sizes
original_size = len(json_string.encode("utf-8"))
compressed_size = len(compressed_data)

print(f"Original Size: {original_size} bytes")
print(f"Compressed Size: {compressed_size} bytes")
```

### Output
The message has now grown due to the GZIP headers size.
We have now spent CPU compressing but the message is larger!
```
Original Size: 60 bytes
Compressed Size: 70 bytes
```

# Intro:  binary message format

Many free and open source libraries exist which 
make it nearly trivial to use this kind of pre-agreed
messages like our `a,b,c` example, effectively pre-compressing
the messages.  These message formats accomplish this 
goal by using a message schema.  The schema is agreed 
upon and shared between the systems exchanging the
messages.

For example if we define the Protobuf Schema for a User
it would be this schema `user.proto` file:
```protobuf
syntax = "proto3";

message User {
  string first_name = 1;
  string last_name = 2;
  string birthday = 3;
}
```
This schema is then loaded by both programs and
used to serialize and de-serialize the messages.

In doing so the wire format is much more compact
by avoiding sending the keys and instead sending
the integer values from the schema file to identify
the fields of the User.

### Binary wire format for Protobuf
When using protocol buffers (protobufs) 
the wire format is binary.  

Protobuf Binary will use UTF-8 to 
encode the characters for our user Chris
when put on the wire but use the schema and 
numbers in the schema to achieve the minimum
size of data sent; for example:
```
b'\n\x05Chris\x12\x06Rodier\x1a\x081/1/1900'
```

### Python Code
You must download and install protoc in your path.
This is a trivial download and unzip from the protobuffers
site..
[Protobuf Releases for Protoc](https://github.com/protocolbuffers/protobuf/releases)

Next compile the protobuf from earlier `User.proto`
```shell
~/coding/protoc/bin/protoc --python_out=. User.proto
```
This generates User_pb2.py which should be in the same
folder as this file.  (I am using PyCharm.)
```python
import User_pb2
import sys

# Create a User message
user = User_pb2.User()
user.first_name = "Chris"
user.last_name = "Rodier"
user.birthday = "1/1/1900"

# Serialize the message to bytes
serialized_data = user.SerializeToString()

print(f"{serialized_data}")
# Calculate the size in bytes
message_size = len(serialized_data)

print(f"Serialized User message size: {message_size} bytes")
```
#### Output

```
b'\n\x05Chris\x12\x06Rodier\x1a\x081/1/1900'
Serialized User message size: 25 bytes
```

This binary wire format has the same exact data but is
only **25** bytes.  

From our original **60** bytes
we are now down to 25 bytes.  This is clearly is
savings on the wire of more than 50%.  

Importantly beyond the wire
savings, we have also avoided any CPU compression of GZIP.

The binary wire format for protobuf is like the GZIP
data without needing to zip the keys.  We do not need
to run the compression which saves CPU cost of compressing.

## Binary alternatives

There are many binary message libraries available including:
1. Protobuf:  Most widely used and integrated in gRPC
2. Avro:  From the Hadoop community; used in big data java systems
3. Thrift:  Similar to protobuf but from Meta
4. CapNProto:  From the author of Protobuf, a kind of "2nd protobuf developed outside google."

Above formats use trivial schema like protobuf to define messages
and are ultimately similar.

### Pick one

For your business you do not want to be paying the cost
of mixing and matching message formats.  This is 
a high cost to the engineers who may maintain two alternate
types of messaging systems and formats.

Worse, with two message formats in use, the messages will need to be
translated across the formats.  This is a new
problem you will have *invented* for your own business,
along with the associated costs.

You do not want any time spent by engineers in 
translating in between formats as this is 
your highest cost, but also your highest **opportunity cost**
of what else those programmers could be doing for you,
as a scarce resource for your business.

This kind of message translation layer is generally
not something a programmer will be eager to author
or maintain.  With AI, maintenance has become easier
but there is still costs to doing it this way 
in complexity and the need to maintain this mapping code.

These mapping layers often become the most complicated 
parts of the system;
most systems are primarily message middleware.

### Protobuf preferred

Comparing to above, Protobuf is generally preferred for a few key reasons:
1. **gRPC** exists for Proto and is powerful for making fast messaging systems
1. Google supports gRPC and Protobuf; also uses Protobuf internally along with the pre-cursor to gRPC

For this reason protobuf enjoys the most cross language support.
When you use protobuf you can be confident that every major and minor
language you may ever want to use in your organization is supported 
is one advantage.  Usually the maintainer of the system
in the major languages like Java and C++ is Google itself which 
leads to an overall high quality experience with the tech.

### Avro comparison

Avro is one with strong Java community support.
Avro until recently lacked anything to compete with gRPC
and therefore was not a serious candidate for most 
businesses.  Avro was chosen by Confluent as the first
format for their binary schema registry and this
has made Avro more popular for usage with Kafka; however,
[protobufs work with this registry](https://docs.confluent.io/platform/current/schema-registry/fundamentals/serdes-develop/serdes-protobuf.html) 
as it has been added by Confluent in recent years, 
although originally Avro was the only choice.

#### Avro Cons (major)

Avro lacks the kind of service and client code
generation facility of gRPC.  gRPC will generate
a fully working server which uses protobufs by
defining the calls in gRPC IDL.

### gRPC IDL

This kind of gRPC idl is authored and then passed
to the gRPC compiler in Java, C++, every major 
and almost every minor language in use.

```protobuf
syntax = "proto3";

import "User.proto"; // Assuming you have a user.proto file for the User message.

// Define a gRPC service called UserService
service UserService {
  // RPC method to create a new user
  rpc CreateUser (User) returns (UserResponse);
}

// Define a message for the response
message UserResponse {
  string message = 1;
  // You can include additional fields in the response if needed.
}
```

After you compile this to Java for a server and client
you simply connect using a host and port you choose
and away you go with binary message formats, clients
and servers.

This approach of gRPC directly between microservices
is generally preferred for the consideration of 
cost benefits of the alternatives for generating
binary message servers; primarly cost.  There is
no other system like gRPC and Protobufs with this
degree of support, well documentd and easy to use.

### Additional benefits of Protos

Another key benefit of Protos is the compile time 
safety.  The code generated forces any compile safe
language to use field names which are known to exist
and work *at compile time*.  When you give up this 
compile (or in other languages **lint** time safety)
you are losing a key build time verification and pushing
it to runtime.  Advocates of unit testing may claim
that you should anyway have 100% test coverage.
Suffice it to say, making this claim is not reasonable 
in a startup or most business environments, and is
more of a hypothetical than practical argument without
any regard for the reality of costs of unit testing.

Anyone who has observed the relative cost and complexity
of a very large distributed python system vs. the same 
in Java can note; while difficult to measure, once
a system becomes large the Java system is measurably
easier and cheaper to maintain and operate due to compile
time safety.  Like the [**Tao Te Ching**](https://en.wikipedia.org/wiki/Tao_Te_Ching) says; the value of compile time
safety can not be properly communicated with words, it can only be *experienced.*

### Cons of Protos

The one con of Protos compared to Avro is that Avro
allows for backwards incompatible schema evolution.
In practice there is never any system where you 
can make safe, backwards incompatible changes, with a
real rollback plan.  Therefore, while a neat feature of Avro
in practice you will never be able to use backwards
incompatible schema evolution.

### Thrift: Not really a contender

While I have reviewed and performance tested Thrift
I have never observed it in finance or any of the 
many companies I have worked at, or across any
of my friends and the companies they have collectively
worked across.  Looking at Thrift releases, the frequency
is not as extensive and it looks to be less maintained.

We can see 60k stars for Protobuf on GitHub and
13k stars for Thrift.  Ultimately they are similar
with more people having used gRPC and more major projects
using gRPC for example HashCorp using it as part
of their core cloud products and other notable major
open source projects taking a dependency.  gRPC
is by far the most familiar and often used in industry.

# Zero-Copy frameworks

## CapN Proto vs. Protobufs (and Flatbuffers)

CapN Proto seems to be developed by the original
protobuf author.  CapNProto notably has implemented
*'zero-copy'* messaging.  In summary, CapN Proto
can be slightly faster because it was designed with
'zero-copy' from the ground up.

## What is zero-copy

When a message is received by the network card these
bytes are sent along into the system.   Message
systems including CapN Proto and Aeron are designed
to use these exact byes and not copy them again
(not even once), to make the message which is used
by Java, C++ or whatever system is in use.  In 
many high speed systems the very copy of this network
buffer into bytes which are used for each field become
the bottleneck.  Imagine you are running an ad-server
receiving half the internet as impressions.  Each
one of these ad-impressions coming from the internet
must be understood by Java or other code.  CapNProto
and Aeron will use the original network bytes without
creating additional buffers, saving time and CPU cost
to "read" these messages into Java objects or C++
structs which then understand the message and can 
operate on it; for example reading my last name
and looking up my purchase history to decide what Ad
to show me.  To do this the system must read my name
and compare it to other names in a cache in memory.
CapNProto will not copy the name bytes "Rodier" into
a Java object field "lastName"; instead it will define
bytes and then mark the existing memory buffer as the last name
field.  In Protobuf and other formats, a Java object
like "User" is created, and then the fields last name
and first name.  These fields are then *populated* from
the bytes coming off the network; instead of using 
the original bytes.  This is a copy.  This means 
Protobuf can be thought of as a **"one copy"** messaging
framework using binary messages. 

If we ask AI, ChatGPT provides a nice overview as well.

But if you have read this far, I have an important
observation which AI will not tell you... and
this is a critical and not often considered observation...

### ChatGPT on CapNProto vs Protobufs

ChatGPT provided a nice summary, saving me some typing here.  
It is a detailed topic and this was good treatment:

---
Yes, Cap'n Proto is designed to be a zero-copy data serialization protocol, much like Protocol Buffers (protobufs). Both Cap'n Proto and protobufs aim to efficiently serialize and deserialize data with minimal memory allocation and copying.

Here's a comparison of Cap'n Proto and protobufs in terms of zero-copy serialization:

#### Zero-Copy Serialization:

**Cap'n Proto:** Cap'n Proto was designed from the ground up with zero-copy in mind. It uses a schema-driven approach to generate code that directly operates on serialized data without intermediate buffers. This means that when you serialize or deserialize data using Cap'n Proto, you can work with the data directly without copying it into separate memory buffers.

**Protocol Buffers**: Protocol Buffers, while efficient in terms of serialization and deserialization, may involve some copying of data. The generated code typically involves copying data from the serialized format to a separate data structure in memory.

#### Performance:

**Cap'n Proto:** Due to its zero-copy design, Cap'n Proto can offer better performance in situations where minimizing memory allocation and copying is critical, such as high-throughput messaging systems and network protocols.

**Protocol Buffers:** Protocol Buffers are also efficient but may involve some memory copying, which can introduce a small performance overhead compared to Cap'n Proto, especially in scenarios where data serialization and deserialization are frequent.

#### Ease of Use and Ecosystem:

**Cap'n Proto**: Cap'n Proto is designed to be efficient and low-level, which can be a double-edged sword. While it offers excellent performance and zero-copy capabilities, it may require more manual memory management and lower-level code.

**Protocol Buffers**: Protocol Buffers are known for their ease of use and a rich ecosystem of language support and tooling. Protobufs offer a more user-friendly API and extensive language bindings, making them a popular choice for many projects.

In summary, both Cap'n Proto and Protocol Buffers are capable of efficient serialization, but Cap'n Proto shines in scenarios where zero-copy and maximum performance are critical. Protocol Buffers offer a more user-friendly experience and a broader ecosystem of language support. The choice between them depends on your specific use case and requirements.

----

## The Field Observation: CapNProto vs. Proto

In some organizations CapN proto will be chosen and 
the developers will say, we have read the marketing
and CapNProto is faster.  You can read the docs on
why it is faster.  And you may read the docs and think,
why does everyone not use CapNProto if it is faster?

There are two minor reasons and one key major reason
which may not have been understood.

### Lack of gRPC for CapNProto

Fewer developers know CapNProto and there is marginally
less support.  CapNProto lacks gRPC to define remote 
IDL interfaces and generate servers; this is a big one.

To do businesses with gRPC from an engineering cost
perspective vs. doing it without could be 30% or
more overall engineering cost.  In almost every situation
there is no discussion; you need to save the engineering
costs against a small performance win, and use 
Protobufs because of the gRPC cost win.

### The observation: What is the performance win of CapNProto?

You may believe that because the CapNProto will be faster
due to the marketing.  It has zero-copy, therefore, it must
be faster right?  We want the fastest!  

This would be totally incorrect as a belief.

How could this be, when it is zero-copy vs. one copy for Protobuf?

#### Null treatment in CapNProto and other zero-copy

When you are doing zero-copy in CapNProto and you have null 
fields, these nulls are sent for the fields **on the wire.**

Therefore, when you are sending sparsely populated messaging
the CapNProto messages will be larger than the Protofuf messages,
**because** protobuf messages will *not* send nulls on the wire
and instead the wire will have fewer bytes.  With a smaller
network message the win of zero-copy is almost always
eliminated due to the higher message size with the nulls
serialized.  Having every field defined makes the zero-copy
able to chop up the byte buffer into a struct without any
instructions other than offsets, the fastest possible way.
The cost is that null fields must also be represented.
For any scarcely populated message this additional network overhead in CapNProto
offsets any performance gained by zero copy.

If you choose CapNProto thinking you will be faster, 
you will have actually incurred 50% more engineering cost
due to the 1) lack of gRPC and 2) less easy to use CapNProto.
50% time across a decent size engineering staff amounts to 
millions of dollars.  

You may buy me a Ferarri now for preventing
this mistake at any organization which reads this.  

I would like it in Red.
---

---

Hopefully after this reading, you understand and can actually debate the binary message
format discussion with your team with an informed understanding
and perspective, and the tradeoffs.  You will not be subjected
to some smarty-pants engineer who says "we should use CapNProto, because it is faster."

This brings us to Aeron in conclusion, for completeness.

## Aeron (Martin Thompson)

Aeron is true zero copy for Java and is zero copy
along with being essentially the fastest way for Layer 7
to send messages between two systems or batches of messages.

Aeron is used in High Frequency Trading (HFT) and other
highly speed sensitive environments.  The cost of programming
in Aeron to Protobufs is about 2-3x as expensive from
an engineering cost perspective.  It is appropriate only
if your business success depends on it.  It may also be
appropriate for connecting systems where tens of millions 
may be saved in CPU vs. the engineering cost of five million
in engineering time.  Also notable about zero-copy for
large systems (tens of thousands of machines) is that
if the messages are not sparsely populated, there is
an opportunity to save a considerable amount of CPU cost
which is energy savings and is Earth friendly.  You get 
to both go much faster for your clients, and save some Earth.
We call this a win-win (as long as you can pay the expert Aeron message engineers,
and they cost less than the CPU savings.)

## Flatbuffers (also Google)

Flatbuffers is zero-copy but requires much more overhead
to program in than protobuffers.  Flatbuffers is much less elegant
to use.  Yet where performance is paramount, Flatbuffers has become a standard,
due to the balance between engineering cost and latency benefits
being in a sweet spot.

What stands out to me was the use of Flatbuffers for Android
messaging in the Facebook (Meta) apps.  This article has
excellent treatment of the advantages of Flatbuffers:

[Flatbuffers for Android at Facebook](https://engineering.fb.com/2015/07/31/android/improving-facebook-s-performance-on-android-with-flatbuffers/)

Flatbuffers notably *does not* serialize nulls.  
The flatbuffer wire format will be more efficient than CapNProto.
Avodinging nulls on the wire comes at a small cost 
during deserializing the messages; which is to use virtual
pointers in the messages.  

This is the right trade-off to make. In
a world where most messages are sparsely populated and contain
many nulls, you want the benefit of smaller network messages.  

In real world applications with sparely populated messages, 
Flatbuffers will be faster in practice than CapNProto, due to 
avoiding null seraliztion on the wire.

Flatbuffers uses the familiar Protobuf schema.
This makes flatbuffers an ideal choice for an organization where generally,
being very fast with protobuffers is preferred. Yet flatbuffers allows
you to tune it up to 11 (zero-copy) with Flatbuffers when and if
there is a cost benefit.  The Facebook Android app speed is one place
where a few million in engineering costs were worth the latency win
for Facebook on their Android app, as an example. Not because
it is better or cool tech; because they make more money
when the system is X faster at Y engineering cost, and X > Y.

Flatbuffers are not difficult to use but are far more cumbersome than Protobuffers.

Compared to Aeron I would call Flatbuffers highly-usable
and a sweet spot for super high performance with reasonable
engineering costs.  My belief is that Flatbuffers are a sweet
spot for low latency trading in finance; short of HFT cases where Aeron
would be necessary to do business.

# Conclusion

You have made it this far on the journey.  
It is an important one to go on.  Why this important?

Understanding this discussion can help you prevent having
multiple message formats and technologies in use in your shop;
and help you standardize on one and only one.  This makes
the engineering cost much smaller and the code much easier 
to navigate.

The importance of this discussion is what led Google to
invent Protobufs and to also mandate that
it be the primary message format.  Further, there is
one project of Protobufs where one set of messages are defined.
This makes it easy to add a field or change the domain model
as your business evolved, a topic I cover in [Domain Model](2023-09-17-domain-model-importance.md)
The combination of using Protobufs for everything along with
Domain Model being enforced across all systems is one 
important factor in the overall success of Google.

To handle this problem well outside of Google
we must read and appreciate these concerns.
There are many subtleties which are difficult to 
grok at face value.  The hope is this article has
cleared up the conversation and the important pieces,
and highlighted the critical subtleties.

The combination of these two discussions is a powerful
organization technique for building enterprise software systems, 
from both a message efficiency format but more importantly
an engineering cost and productivity perspective.

You get compile time safety and much faster system performance
when using Protobufs; which are free as in beer to use.

Why would you *not* use them?

## Don't know what to do?

If you don't know where to begin:
1. Make a repository for only ".proto" fies.
2. Define your business domain in there
3. Build your services with gRPC
4. Use [gRPC Gateway](https://github.com/grpc-ecosystem/grpc-gateway) to offer JSON to your front end ... or...
5. better.. [Use Protobufs Directly in the Browser](https://protobufjs.github.io/protobuf.js/)

# What about retro-fitting?

If you already have a JSON based system but want to 
slowly pivot to doing things one better (safer at compile time,
also, faster at runtime) then OpenAPI YAML and Generator
are an interesting alternative.

Using OpenAPI and the [OpenAPI Generator](https://openapi-generator.tech/) project, you can
define APIs in YAML and then compile them in either protobufs,
Avro or others.  OpenAPI has the advantage that you
can use it for your Public facing API, directly without translation, 
if you need a public API in JSON.

But it seems to me we should prefer to offer a protobuf API
for a business as it makes generating clients in any
language trivial.  gRPC/Protobufs also eliminates a major
class of Ambiguity which is the challenge of defining
APIs in REST; HTTP and JSON, was not meant for all messages!
