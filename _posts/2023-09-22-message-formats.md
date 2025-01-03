# Zero-Copy vs. Binary vs. JSON: A Comparative Analysis of Efficient Message Formats

Message Format is a critical OSI Layer 7 (Application) decision.  
For any reasonably large messaging
system this decision can determine millions of dollars
of CPU and engineering time.

The results and reasons seem to be poorly understood
making the primary choice the ubiquitous JSON.

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
Each character must be serialized to send this message on the wire.
When we serialize the JSON above we are 
sending more characters for the
JSON key names than the actual data.  

In most message cases this overhead is minor.
The latency cost may not be critical. We may not be
sending the records over the wire often.  

While this is the case for many businesses when they start, it
is rarely the case for a mature business that message
speed and size is unimportant.

We can note that the keys of the JSON are duplicated
for each record in their entirety (first, last, Birthday.)

It will be almost twice as expensive to send along
these long keys with each set of user data.  What if we replace
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
work and complexity to manage this format.  

Already this trivial encoding is something 
similar to [Message Pack](https://msgpack.org/)

## Message Pack
We can take the original JSON array use
a library called Message Pack to serialize it.  
The wire format is like our `a,b,c` compression.

Message Pack will put the keys first and then use
tokens for the keys to avoid copying the entire strings
for every message in a form of message compression.

For example, our message as Message Pack serialized:
```
\x92\x83\xa5first\xa5Chris\xa4Last\xa6Rodier\xa8Birthday\xb41/1/1900\x83\xa5first\xa3foo\xa4Last\xa3bar\xa8Birthday\xb412/31/2022
```
MessagePack is effective for this one problem.
The keys have been compressed by sending 
only once for a list of the same messages.  

But for sending one message the size is almost identical:
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

### What have we saved with Message Pack? (Not much!)

This python code prints:
```
JSON Size: 60 bytes
MessagePack Size: 43 bytes
```
This experiment revealed we use **43** bytes with message pack
vs. **60** bytes for JSON.  

We note the **29%** improvement on size and **17** bytes saved.
But we have though used an additional library across our code - this
does not seem to be worth complexity cost.

MessagePack is a case where the benefits are not worth the investment.  

The cost is introducing MessagePack everywhere
in your system, as a library, upgrades and maintenance, along
with the learning curve for engineers.

MessagePack also lacks other
critical and helpful messaging features which we will now explore.

### GZIP Compression

It is simpler to enable GZIP compression compared with  
an additional library dependency on MessagePack.

GZIP is baked into most service frameworks and also modern browsers.
GZIP is easily turned on as a feature of a Java server
where the browser will instantly interpret and decompress the GZIP
for you without even changing the JavaScript code.

This use of compression is an easier win than
using MessagePack.

### Cost of GZIP = CPU

The cost of the GZIP compression is CPU cost.  
You could also experiment with Snappy.
Both of these are efficient and the CPU cost is tiny
for a small number of messages. The CPU can compress
small messages almost instantly.  Sending smaller messages
is always faster.  A question exists and could be unique
to your network, CPU and business case; is compressing and sending
fewer bytes faster or slower than sending the bytes
without compression?  In the cloud the network is generally
fast enough where it is a small win to compress but
not significant enough for small messaging cases.
The CPU speed at 4GHZ though makes it typically a win
these days of some small degree although in some cases
with certain CPUs it will be slower to compress and send.

The win on any machine
is small enough that it should be tested first and not 
assumed it will be faster compressed as it varies
for the messages and the compression used.  

When you compress you may be stealing valuable CPU from
your application.  While compression uses very little
CPU for small messages, for large messages and large
arrays of large messages it is measured in tens of 
seconds to compress and decompress the messages - a high cost.
There is an important cost-benefit to using or avoiding compression.

What if this CPU cost could be eliminated.  What if we 
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

The output looks deceiving but in fact due to GZIP headers,
the message has grown larger by 10 bytes.

The larger size also achieved with an additional CPU cost to compress.
```
Original Size: 60 bytes
Compressed Size: 70 bytes
```

For cases which are not contrived the message will be smaller.
This example illustrates the importance of testing with your 
own systems and data.

# Intro:  binary message format

Multiple open source libraries exist for binary message formats.

The most popular binary message format is Protocol Buffers.

A pre-defined message schema is used instead of the string keys.

The schema is agreed upon and shared between the systems exchanging the
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

In doing so the wire format is much more compact.
It is more compact in not sending the string keys.
Protobuf sends the integer values from the schema file to identify
the fields of the User.

### Binary wire format for Protobuf
When using protocol buffers (protobufs) 
the wire format is binary.  

Protobuf Binary will use UTF-8 to 
encode the characters for our user
object "on the wire" but use the schema and 
numbers in the schema to achieve the minimum
size of data sent; for example:
```
b'\n\x05Chris\x12\x06Rodier\x1a\x081/1/1900'
```

### Python Code
You must download and install `protoc` in your path
for the next example.

This is a trivial download and unzip from the protobuffers
site..
[Protobuf Releases for Protoc](https://github.com/protocolbuffers/protobuf/releases)

Next compile the protobuf from earlier `User.proto`
```shell
~/coding/protoc/bin/protoc --python_out=. User.proto
```
This generates User_pb2.py appears in the same
folder as this file.  (I am using PyCharm.)

Next we can use the following python program to
import the generated code
and send a User with protobufs and Python.

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
we are now down to 25 bytes.  This saves **58%**.

Beyond the wire
savings, we also avoided any CPU compression of GZIP!

The binary wire format for protobuf is like the GZIP
data without needing to zip the keys.  Protobufs avoids
the compression which saves CPU cost of compressing keys.

### Data structure is well known

The schema file will give IntelliJ or PyCharm
awareness of the structure of the User.  
With JSON we generally will lack such schema.
There are major benefits to having a well known 
sturcutre and format of fields in your code
with this code awareness.  This in itself is 
in my view reason enough to use Protobufs for all messaging.
Combined with the performance benefits, protobufs
becomes a no-brainer for all but trivial applications.

## Binary alternatives

There are many binary message libraries available.

The four most relevant to examine are:
1. Protobuf:  Most widely used and integrated in gRPC
1. Avro:  From the Hadoop community; used in big data java systems
1. CapNProto:  From the author of Protobuf, a kind of "2nd protobuf developed outside google."

Above formats use similar schema to protobuf for message definition,
and are ultimately similar.

### Pick one ?

For your business you do not want to be paying the cost
of mixing and matching message formats.  This is 
a high cost to the engineers who may maintain two alternate
types of messaging systems and formats.

Worse, with two message formats in use, the messages will need to be
translated across the formats.  This is a new
problem with the associated costs which is avoided by using only one
technology.

This kind of message translation layer is generally
not something a programmer will be eager to author
or maintain.  With AI, maintenance has become easier
but there are still high costs  
in complexity and the need to maintain this mapping code.

These mapping layers often become the most complicated 
parts of the system;
and most systems are primarily message middleware!

### Protobuf preferred

Protobuf is generally preferred for a few key reasons:
1. **gRPC** exists for Proto and is powerful for making fast messaging systems
1. Google supports gRPC and Protobuf
2. Google uses Protobuf and a flavor of gRPC internally for all messaging

For this reason protobuf enjoys the most cross language support.
When you use protobuf you can be confident that every major and minor
language you may ever want to use in your organization is supported 
is one advantage.  Google maintenance for the language plugins 
leads to a high quality developer experience.

### Avro comparison

Avro enjoys strong Java community support.
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

gRPC makes a service interface IDL available.

The interface IDL is authored and passed
to the gRPC compiler in any major language.

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
binary message servers; primarily cost.  There is
no other system like gRPC and Protobufs with this
degree of support.  

gRPC is well documented and easy to use.

### Additional benefits of Protos

Another key benefit of Protos is the compile time safety.  

The code generated forces any compile safe
language to use field names which are known to exist
and work *at compile time*.  When you give up this 
compile (or in other languages **lint** time safety)
you are losing a key build time verification step, 
and delaying it to runtime.  

Anyone who has observed the relative cost and complexity
of a very large distributed python system vs. the same 
in Java can note; while difficult to measure, once
a system becomes large the Java system is measurably
easier and cheaper to maintain and operate due to compile
time safety.

### Cons of Protos

The one con of Protos compared to Avro is that Avro
allows for backwards incompatible schema evolution.
In practice there is never any system where you 
can make safe, backwards incompatible changes, with a
real rollback plan.  Therefore, while a neat feature of Avro
in practice you will never be able to use backwards
incompatible schema evolution.

# Zero-Copy frameworks

## CapN Proto vs. Protobufs (and Flatbuffers)

CapN Proto is developed by the original
protobuf author (post Google.)

CapNProto notably has implemented
*'zero-copy'* messaging.  In summary, CapN Proto
can be slightly faster because it was designed with
'zero-copy' from the ground up; however, zero-copy
is not always faster.

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
to read and then copy these messages into Java objects or C++
structs which then understand the message and can 
operate on it.  

CapNProto will not copy the name bytes "Rodier" into
a Java object field "lastName"; instead it will define
bytes and then mark the existing memory buffer as the last name
field.  In Protobuf and other formats, a Java object
like "User" is created, and then the fields last name
and first name.  These fields are then *populated* from
the bytes coming off the network via copying; instead of using 
the original bytes.  This is a copy.  This means 
Protobuf can be thought of as a **"one copy"** messaging
framework using binary messages. 

ChatGPT provides a nice overview as well.

But if you have read this far, I have an important
observation which AI will not tell you... first let us see 
what ChatGPT thinks.

### ChatGPT on CapNProto vs Protobufs

It is a detailed topic and this was good treatment from ChatGPT.

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

In some organizations Cap-N-proto will be chosen due to marketing.
You can read the docs on Cap-N-Proto and see it is faster.  

And you may read the docs and think,
why does everyone not use Cap-N-Proto if it is faster?

There are two minor reasons and one key major reason.

### Lack of gRPC for CapNProto

Fewer developers know Cap-N-Proto and there is marginally
less support.  Cap-N-Proto lacks gRPC to define remote 
IDL interfaces and generate servers; this is one major reason.

To do businesses with gRPC from an engineering cost
perspective vs. doing it without could be 30% or
more.  In almost every situation
there is no discussion; you need to save the engineering
costs against a small performance win, and use 
Protobufs because of the gRPC cost win.  Before you 
incur a 30% overhead on development time you must
understand if there is even a win in CapNProto, and,
how big a win?

### The observation: What is the performance win of CapNProto?

You may believe that because the CapNProto will be faster
due to the marketing.  It has zero-copy, therefore, it must
be faster right?  We want the fastest!  

But it may not always be quicker; and it is always more expensive to use
from a programming time perspective.

How could Cap-N-Proto be slower when it is zero-copy vs. one copy for Protobuf?

#### Null treatment in CapNProto and other zero-copy

The zero-copy ability of Cap'n Proto 
comes at the cost of representing nulls.

CapNProto messages can be larger than the Protofuf messages,
**because** protobuf messages will *not* send nulls on the wire
and the wire may have fewer bytes.  

Having every field defined gives zero-copy
the ability to chop up the byte buffer into a struct without any
instructions other than offsets; this is the fastest possible way.
The cost is that null fields must also be represented.

*For any scarcely populated message this additional 
network overhead of representing nulls in CapNProto
may offset any performance gained by zero copy.*

If you choose CapNProto thinking you will be faster, 
you may have actually incurred 50% more engineering cost
due to the 1) lack of gRPC and 2) less easy to use CapNProto.

50% time across a decent size engineering staff amounts to 
*millions of dollars.*

Note: Recently Cap-N-Proto has introduced Cap-N-RPC but
this is less mature and widely known than gRPC.

Only by testing your specific message cases and with careful
analysis of sparsely populated messages could you know
if Cap'N'Proto will be faster or easier to use than Protobufs.
If there is any debate, protobufs should be favored due to 
engineering time as an additional cost to use Cap'N'Proto.

## Aeron (Martin Thompson)

Aeron is true zero copy for Java and is zero copy
along with being essentially the fastest way for Layer 7
to send messages between two systems or batches of messages.

Aeron is used in High Frequency Trading (HFT) and other
highly speed sensitive environments.  The cost of programming
in Aeron to Protobufs is about 2-3x as expensive from
an engineering cost perspective.  Aeron is appropriate only
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

## Flatbuffers (also Google, cousin of Protobuffers)

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

Avoiding nulls on the wire comes at a small cost 
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
being very fast with protobuffers is preferred. 

Yet flatbuffers allows you to use zero-copy when and if
there is a cost benefit.  

The Facebook Android app speed is one place
where a few million in engineering costs were worth the latency win
for Facebook on their Android app, as an example. Facebook are 
X more successful when the system is 
Y faster at Z engineering cost, and X > Z.

Flatbuffers are not difficult to use but 
are far more cumbersome than Protobuffers.

Compared to Aeron I would call Flatbuffers highly-usable
and a sweet spot for high performance with reasonable
engineering costs.  

Flatbuffers are a sweet spot for low latency trading in finance; 
short of HFT cases where Aeron is strictly necessary for 
single digit microsecond performance (mics, not millis.)

# Conclusion

You have made it far on an important journey!   

Understanding this discussion prevents introducing
multiple message formats and technologies in use in your shop -
and helps you standardize on one preferred standard.  

Using a standard for messaging reduces
the engineering cost along with improving code.

The importance of this discussion led Google to
 author Protobufs and mandate
it as the primary message format.  

Importantly across Google, 
a single project of Protobufs defines the business domain.

This centralization allows engineers to field or change the domain model
as your business evolves, and *it changes in only one place.*

I discuss this importance in [Domain Model](2023-09-17-domain-model-importance.md)

One could argue (one does) 
.. this combination of approaches is critical to Google's success;
of 1) using Protobufs for everything, along with 2)
Domain Model being enforced across all systems.

To handle this problem outside of Google
we must read and appreciate these concerns.

There are many subtleties which are difficult to 
grok at face value.

The combination of these domain model along with the correct message format is paramount
to an expert software engineering practice. 

Together they solve two key problems having an exponential effect;
 both a message efficiency format but more importantly
an engineering cost and productivity perspective.

You get compile time safety and much faster system performance
when using Protobufs; both are free.

Why would you *not* use them?

## What to do?

If you don't know where to begin:
1. Make a repository for only ".proto" fies.
2. Define your business domain in there
3. Build your services with gRPC
4. Use [gRPC Gateway](https://github.com/grpc-ecosystem/grpc-gateway) to offer JSON to your front end ... or...
5. better.. [Use Protobufs Directly in the Browser](https://protobufjs.github.io/protobuf.js/)

## What about retro-fitting?

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
class of ambiguity which is the challenge of defining
APIs in REST.

HTTP and JSON, was not meant for all messages!

## Appendix

At Layer 4 (Transport) in the OSI model
you first choose TCP or UDP; but this is almost
always TCP due to error correction (along with de minimus
gains of using UDP over TCP coming at a high complexity cost.)
