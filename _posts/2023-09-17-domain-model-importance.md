# Domain Model: Your Business Blueprint

A domain model is a package of classes and interfaces that
represents your business domain in code. This software model
of your business domain becomes a custom language by which
your systems communicate and store information.

As the central representation of your business, your Domain
Model **IS** your business. The success and health of your
software practice have a life-or-death dependency on the
representation of this domain model; both its accuracy and
how it is implemented.

When done correctly, the representation is in one single
place, and the format is one that can save compact wire
format versions, typically binary. It allows for polyglot
development and, in certain cases, is 'zero-copy'.

If you can capture all of the above in one Domain Model,
you have achieved a kind of nirvana in software which makes
modification and ownership inexpensive for the engineers,
along with simultaneously making the software up to 10x
faster at runtime than alternatives like JSON.

## Prior-Art and Books

Many books are written on "Domain Driven Design." The one I
studied in school is the "Larman UML" design book. Major
portions of this book are towards a Domain Model but capture
a larger portion of the system design process. Larman begins
with writing business cases and continues into designing
the system and unit testing.

[Applying UML and Patterns by Craig Larman](https://www.craiglarman.com/wiki/index.php?title=Book_Applying_UML_and_Patterns)

The sample chapter 6 is an excellent introduction to "Use Case"
design modeling, which informs Domain Driven design. It begins
with writing detailed business cases which are merely steps
that say "user does X, system responds Y." After this writing
of business cases, the X's and Y's can be trivially identified
as nouns and verbs. These and their relationships to each
other are your Domain Model.

You have succeeded in making a Domain Model of your system
when you have zero "synthetic" objects in the software
representation which are "types" (classes) which have no
direct mapping to the business domain. This is what is
considered a 'clean' software model of a business domain.
Synthetic types should only exist to improve latency. 
If these object exist beyond improving latency
then there is a flaw in the design.

## Accuracy of the Domain Model

The domain model accuracy is critical to your success.

The question of inheritance vs. composition is one of the
most critical. Often times inheritance is used when
something really "is not" something else; it merely
should "have" something else on it, like audit information.

This type of mistake can make the software much more
cumbersome to work with and is often unnecessary.
Composition should always be preferred. It is rare
you actually have a situation where "a Truck is a type of
Vehicle" (inheritance), or where this relationship is
important enough that it should dictate the shape of your
model. In any banking application, there is not a real base
class "thing which has an account" which everything should
inherit from. There is no "base account" in the universe.
There are things which have accounts; and then, the account
can be attached to them using composition. This is a common
mistake of using inheritance to capture a relationship which
is not truly there as a means to propagate one field onto
a domain model.

## Domain Model as Data

Eventually your domain model must be modeled as data stored
somewhere; typically the domain model is persisted in a
database.

When your domain model is persisted in a database it is
first represented in an ERD; an Entity Relationship Diagram.

The ERD captures specific relationships as data itself.
An important concern is having audit on every table;
who edited the data and when. 2nd is the key on every piece
of data. When you make this ERD you typically use
synthetic primary keys for the size of the storage of the
"long" datatype in a database. 

In this phase of ERD
development you are no longer doing domain modeling but
rather translating your objects into the tables you are
designing. Temporality and bi-temporality should be
considered when making an ERD; this is typically the
correct way to approach user data when you model data.
Without temporality, you must audit the data in a separate
set of tables you are also authoring at double the cost
and more than triple the complexity. Now you have invented
the need for an entire alternate system of data storage
at double the cost of engineering; and a third system to
translate between the two. You would be surprised if you
found this mistake made to the tune of millions of dollars
of engineering time in many large software shops. But you
will; temporality of data is not taught in school, nor the
correct usage for all user-created data artifacts for audit.
This one software note will save you millions of dollars
a year in engineering costs. But if you work with me it
costs you only a beer.

### Domain Model is life

The hallmark of a strong software system is a project which
is independent and is exclusively the domain model.

The hallmark of a software engineering practice is that
all domain models are in one well-organized domain model
project. This takes the single domain model approach to
the level of an entire organization. Once you do this the
chances of your organization being dramatically more
successful than your competitors are high; you will be
up to 10x faster at making changes to your business at
up to 1/10th the cost or less. Taken to an extreme is to
have your entire business domain and all business domains
in one package. You can then choose a representation which
is easy to translate into any language and is sent
rapidly 'on the wire.' This is not only easy to do but
has already been made trivial.

### Protobufs

The preferred format for a domain model should generally be
'Protocol Buffers' from Google, which is open source. In my
view, without ever having worked at Google, one of the key
reasons Google is almost exponentially more successful at
software is because of this domain model principle, employed
"at scale" at Google. And then they open sourced it. Still,
the principle and value are not understood, despite Google
using it internally. In a sense, it is the greatest software
secret of all time, and yet, it is right out in the open.

It is like knowing the play of chess master yet not being
able to repeat it when you yourself play. But this is not
chess, and you can play like a chess master if you
understand this value. The cost to do it this way is
minuscule compared to the overall cost savings over the
life of a software system; a few days of one person
against years of wasted engineering time. Despite this
cost value proposition, most engineers would say at the
start of the project 'protobuf is not worth it.'

The lack of usage of proto-buffers in software design
and organizations is due to the lack of the profundity of
the Protobuf approach which captures both Domain Model and
as well a highly efficient binary implementation on the
wire; combined into one representation. In most
organizations who do not correctly practice domain model
or protobuffers, this is the most expensive mistake made;
not in one organization. It is the most expensive lack of
understanding in the history of mankind, in terms of
production software human hours lost. Where it has not
been used, work harder not smarter is employed at scale by
engineers who are ignorant to these profound design
mistakes.

### Unfixable

The management teams are often left to say "it is too late";
or not worth fixing it. Once the opportunity to use the
Domain Model approach is lost you are left with a problem
of unwinding production messaging systems. While the great
"Joel On Software" says ["never rewrite"](https://www.joelonsoftware.com/2000/04/06/things-you-should-never-do-part-i/);
this is one situation where you must consider it. What is
the cost of maintaining and modifying this software
*without* a Domain Model in place?

### Cost of change

How difficult is adding a field to your domain model? This
is a direct measure of how well your software is designed.
Adding a field is the most common operation to a domain
model. The second most common is adding a new "type"
(class or table.)

Here is the test of the design of your system; how expensive
is it to add a new field. How many different files and
projects need to be deployed? Each one of these comes
with a cost; discussions between teams, designs, and
sending the information across the systems. There is a unit
cost to add this to each copy of the domain model.
In larger software groups and systems the cost includes
an alternate group understanding the change and reviewing it.
On top of this, you need agreement about how to add it to each
new system requiring design and review considerations in
large systems. All of this is completely duplicated work
when you have one tidy copy of your domain model in protobufs.

### Software as a Design Game

An incredible point this raises about software is the
importance of design and structuring your system; your
domain model and your data. With a correct understanding
in the design phase, you can reap weeks and years of
benefits after the system goes into production. Once a
system is in production, it takes up to ten times more
effort to test and release it than when it is under
development. This is because once it is in production the
number of business cases connected to your domain model
grows and each of those must be re-tested before releasing
it. This is not only a cost increase but an increase in
risk of changing domain models and data, once the system
goes into production.

The ability to have a profound impact on software design
by spending small investments to do things in a way
which will help you scale your business system in the future
and make it easy to extend your business is an incredible
feature of the software developer's job. After many years
of developing business systems and after years of studying
software design after starting work did the importance
of domain-driven design become completely apparent.

### Absence makes the heart grow fonder

After designing and working on software systems which relied
heavily on domain-driven design, and then inheriting major
multi-tiered systems which did not use domain-driven design,
did the extreme importance of this topic become apparent.
Domain-driven design is arguably the most important feature
of a software system. Only after working on systems where
the domain was copied many times across a large organization,
and looking across the entire organization, has the paramount
importance of the Domain Model become apparent to me.
This was after the 25+ years of coding and studying;
professionally.

### Agreeing on the format for Domain Model

In one situation I observed a second domain model growing
for an important finance problem. The quantitative
team of mathematicians was using C++ while the rest of the
software team was using Java. Protobufs can communicate
with both of them. Yet after debate, the quantitative
team used a C++ domain modeling framework called ["Cereal"](https://uscilab.github.io/cereal/).

This was done after reading the marketing material on Cereal
and reviewing the performance docs at a high level without
doing a deep dive. You can conclude from [this referenced
performance guide](https://github.com/thekvs/cpp-serializers)
that Cereal is faster than Protobufs.

There are major problems with this approach of cursory
evaluation of frameworks. Let us review how wrong this
decision was.

### This ridiculous benchmark

If we look at the benchmark, we notice there is one
repeated field of ints and one repeated field of strings.
There are no business models that look like this.
In a real business model, there are usually between 5 and
30 fields on each object with closer to 30 being the norm
for any important structures. There is a more critical
observation of real domain models; in most cases, the
fields are `null` for most of the messages. For example,
one of the fields will be null on the domain model 99% of
the time until it is important. The question then becomes
what happens to null fields on each of these frameworks
when you have a large domain model with many null fields.

#### The sample code fake domain model = not useful.
```protobuf
message Record {
    repeated int64 ids = 1;
    repeated string strings = 2;
}
```

The result of this not useful and false marketing of Cereal
for C++ is that engineers, even senior ones, are often found
arguing for points that are patently false. When you are
under pressure to deliver, and you make a decision, it can
seem like the only and correct decision to you. But
if you make the wrong decision about a domain model format,
you can end up paying millions of dollars for the mistake.
How is this? You now need to translate between one domain
model using Protobufs in one group and Cereal used by another
group. It will cost you at least $100,000 in software time
to first author the translation layer, and it will be
many times this to debug and maintain it. With the adoption
of AI, these costs will be reduced, but they will never
make up for the lack of a correct and accurate design discussion.

I myself am often wrong; times I have been way wrong
are cataloged forever in my mind as things to avoid, particularly the big ones.

The scary part of design is once you know you may be wrong,
but you believe you are right in a conversation about design.

Once this happens, you enter the realm of philosophy about
software, cost, and the fundamental truth. Then you dig
deeper and deeper until you hit what one friend calls bedrock truth.

The fundamental truth here is that Cereal puts every field,
null or not, on the wire. If you have a domain model
with half-null fields, you lose more than all the time
you have gained on cereal messages that are fully populated.
Without this understanding or even a cursory understanding of
nulls in messages, you have cost your company real engineering
money, defects, and a major time cost to your project.

These kinds of debates and decisions that can lead to
millions in software mistakes are common and can be seen
in all software shops, along with their outcomes, costs,
and benefits, which become obvious over time.

### Debates about software

In conversations with other developers, it helps to have the
same foundations. Otherwise, you have repeated discussions
about facts that are truths of the software formats
and often general engineering truths. There
is little less fulfilling activity than convincing someone
else of a truth they can learn without you, and you are
willing to direct them, but they are not willing to do the
work to learn what you know.

Principal Engineers are asked to write papers to "convince"
others of what they know as bedrock truth. This turns
the art of software development into one of authoring
your fundamental truth in a paper that is consumable
by both business people and engineers. This has become
standard practice in large dot coms. It works sometimes
in major dot coms. This is because they can afford
to have ethereal conversations about the art of software
development and write papers about why things should
be done a certain way. Most businesses do not have this
luxury and need to trust the instincts of their most senior
engineer and take their word for it. This is frustrating,
and also if you do not have a senior enough engineer at the
helm, your entire software effort can go sideways. But
you cannot expect this engineer to write down the bedrock
truth of why all beliefs are practiced; this is the equivalent
of boiling down every experience and every project on the subject
into a digestible summary document. This can be a pleasant
exercise if one is writing a book or a blog on the weekends;
but to do this with the pressure of business competition
is a senseless exercise for any small or medium business.

## How hard is Domain Model

To build a domain model, you first need to know what
the cost and difference are between using Protobufs, its
close cousin Flatbuffers, and what binary vs. non-binary
formats like JSON give you in cost and benefit.
This is only the beginning. To say that Protobuffers have
too high a cost is to lack an understanding of the problems
you can face with JSON on the wire and in general, which are many.
Problems with JSON include the lack of a schema and an expensive
non-binary wire format. While JSON can be compressed and
then compressed, it is the same size; guess what, you must
then compress and decompress it. This involves turning on gzip
compression for JSON, which is, let us say, not a big deal,
but it is a cost. The bigger cost is the CPUs; now you must
run CPUs hot and take the time to compress and decompress every
message. While this is very fast, once you have even a
moderately interesting number of messages; for example 1,000,000, the
cost can become tens of seconds of CPU time to compress the
data, costing you tens of seconds of CPU time and slowing
down your data pipeline by up to ten seconds. With many
finance data pipelines needing to be sub 5 seconds for
effective real-time reporting, you are now already
over your latency budget; time to start over. This need
to start over when you have not properly allocated for
performance is being referred to as shift-left performance,
where you account for the non-functional latency and cost
of a system at the earliest point possible. The reason
for doing this is because of the extreme cost of
underestimating this cost later in the software development
cycle, which in some cases would be a complete re-engineering
at the cost, which during design time can be a two-hour
conversation and research process, usually leading to
using protobufs to avoid the risk of needing it later!






























