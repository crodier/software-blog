# Microservices myths

### Background

Ian Cooper, says it best (Bravo!)

https://www.youtube.com/watch?v=d8NDgwOllaI

Note that AWS already moved Amazon Prime to a Monolith, with great
results. (out of Microservices, and into a Monolith.)

[AWS Move from Microservices to a Monolith](https://medium.com/@anshita.bhasin/exploring-amazon-prime-videos-architecture-migrating-from-microservices-to-monolith-for-aacbf9fabc73)

Defintion of Modular Monolith:  https://www.milanjovanovic.tech/blog/what-is-a-modular-monolith

Google moving to Modular Monoliths:  

https://dl.acm.org/doi/pdf/10.1145/3593856.3595909

### Executive summary

Use ["Modular Monolith"](https://www.milanjovanovic.tech/blog/what-is-a-modular-monolith)
, and modules of code.  Avoid microservices as a method of last resort.

If you need to improve on "release velocity", then you
can make multiple independent pipelines.  Only after finishing
a full QA in an independent pipeline, 
can a team 1) merge to trunk and 2) release. 

This way, only when a team was set to go to prod, can they merge
to trunk.  You can take it further, and make them deploy
to a canary for a time first, before a merge to trunk

This is the correct and easy approach, 
and also a straightforward and low cost, pipeline DevOps exercise.

Only if you have truly exhausted these two techniques,
can you try to use Microservices to "improve velocity."
The cost of microservices is tremendous, and they are typically 
terrible for organizations.

Exhaust all other approaches, first; find any alternate solutions,
and don't give up.  Microservices are a painful road,
and they can and will lead you to disaster.  

The problems are 1) operational overhead 2) 
the "correctness" challenges, 
and the slow speed of network calls, increasing latency.

You can go much faster development velocity wise with modules
contributing to a Monolithic Service or a Monolith.

Amazon Prime Video moved from Microservices into a Monolith.
Meta has always been monolithic.

Microservices are a fading fad, and like a fad diet, there
were other better ways, traditional techinques to losing weight.

### My own debunking of microservice myths

We will carefully debunk each of these myths.
You may think of us as the "microservices myth busters."

https://aws.amazon.com/microservices/

1. Deployment independence
2. Increased developer velocity
3. Increased scale
5. Agility
6. "Technilogical Freedom"

Services (think "macro services"), when applied correctly
can be highly useful.  First, be aware that when Amazon
use microservices, one of them may be a major tier of Amazon;
the shopping cart for example.  They may be microservices to Amazon
but to you they will be Monoliths.

### Deployment independence

Any cross-cutting concerns, now need to be applied in many multiple
places in your system.  If you are a data system and one
day realize you want [bi-temporality](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://martinfowler.com/articles/bitemporal-history.html&ved=2ahUKEwjF17WBpoiMAxWPlokEHSIMMakQFnoECBcQAQ&usg=AOvVaw2-AEo9x0h6_m8ZflSCCphy)
now you must implement the pattern twice.  The same is true for
all operational concerns; distributed tracing, logging, etc.
You have now violated [DRY](https://www.geeksforgeeks.org/dont-repeat-yourselfdry-in-software-development) 
on a grand scale, for the non-business logic portions of your system.
Indeed, these may be 60% of your system.  Note, I don't believe
in DRY, but since microservices are an inane principle, can 
we use inane principles against it?  I believe we must!

### Increased developer velocity

Splitting up code necessarily decreases productivity.
Now any developer must make changes across; with two PRs, 
two deployments, and worse - all changes to both services
need to be both **backwards** and **forwards** compatible, to each other.

This means, that both services need to be tested, both in the 
presense of, and without the other service.  Why?  One of
the services may need to be rolled back.  You must not have
multiple services which need rollback, simultaneously.  
Now you have much harder code to author, where you need to be
compatible in both directions.  You also need release JIRAs 
twice, and two sets of review of the releases.  What was
before a simple PR to one codebase, is now a ballet of 
different code bases, being released on different schedules.
You will not overcome these costs in your coding velocity -
even if it were you alone working on both services.  This is a 
good test for velocity - ask yourself, self, would I do this
if I were the only person in the company.  If you would not,
then it is probably a bad idea to be doing it even in a large
company.  Why is this true?  Because microservices are only
one way to break up code - did you know?  Modules of software
development have existed since the dawn of time, and distributed
open source teams (linux), have been wildly productive.  

If you already didn't know how to code using 
interfaces and object orientation, then you should not be jumping
to microservices.

### Correctness of a distributed system

[Correctness proofs with Isabella](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://www.youtube.com/watch%3Fv%3D7w4KC6i9Yac&ved=2ahUKEwjAxNnrq4iMAxVMElkFHYctBUIQwqsBegQIFBAF&usg=AOvVaw0UsMAcJ2MKfSMnHEvH5-eD)
is a fun video where the famous Martin Kleppmann of [Data Intensive Apps](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321).
(by the way fun [lecture series](https://www.youtube.com/playlist?list=PLeKd45zvjcDFUEv_ohr_HdUFe97RItdiB).)

If you make a distributed system, you may have [correctness issues](https://martin.kleppmann.com/2022/10/12/verifying-distributed-systems-isabelle.html) 
which you don't know about.  The only real way is to prove you don't have them,
using Isabella referenced above, or the "P" language.

Here is a list of distributed systems papers about bugs, and tests:

https://github.com/asatarin/testing-distributed-systems

What bugs live in the Cloud is a good reference (3000+ bugs.)

Will you write proofs about why your system is correct?
And will you produce a distributed systems test harness, to re-prove
it when you make changes?  The answer is doubtful on both, in
most cases (unless you work at AWS on [Aurora DSQL.](https://aws.amazon.com/blogs/database/introducing-amazon-aurora-dsql/_)  

If you have correctness issues, then you
will have corrupted customer data.  How many of these
can you tolerate, and how many of them are you "ok with"
breaking your customer, your business?

This is a rhetorical question.  The answer should be zero.
If your software doesn't work, someone will find, similar
software which works.  Also, they will tell a million other
people using the internet as a megaphone.  Don't risk it.

### How to be ready for correctness issues

If you want to be ready for distributed correctness issues,
you need a real time reconciliation + correction systems,
in addition to the original microservice.

This is for when data one systems does not agree with data in
other systems.

### Avoid distributed systems unless no alternatives exist

This is why you should avoid distributed systems,
unless absolutely necessary.  Don't do it.

[AWS has their own paper](https://aws.amazon.com/builders-library/challenges-with-distributed-systems/) about the perils of distributed systems.

This is a solid site by AWS, discouraging you from using 
distributed services.

My favorite parts are "Distributed bugs are often latent."
The more common way this is stated is ["fail fast"](https://medium.com/@denhox/the-benefits-of-fail-fast-systems-dc72a665cfb5) in systems.

(here's [another realted post](https://medium.com/@patrickkoss/dont-build-a-distributed-system-if-you-don-t-understand-these-issues-2ae5d60bdad7) like mine I just found)

Another good one is:  "Distributed bugs spread epidemically."

I would call this "you are only as good as your worst microservice."
This means, all your investements in uptime must be repeated, for 
every microservice you build.  You are not measuring these costs,
but as I outline in [Cost of a Microservice](./2023-10-07-cost-of-a-microservice.md)

These new uptime and latency problems are much harder (hard technical problems)
, than the business logic in your code.  

Unless you know what [eBPF](https://www.brendangregg.com/blog/2024-03-10/ebpf-documentary.html) 
is, and how it is being used today, just trust me in saying: 

**Uptime is very complicated and
challenging.  It is much harder than your business message system.**


#### Inter-team issues introduced

You will now to make any changes across the system, need 
to coordinate with another team.  While these will seem rarer,
they will take infinitely longer when you do run into them.

### Increased scale

Every remote call you make takes 1000x more than doing in one process
space, did you know?  Calling a class locally, there is
no seralization, and generally method calls are inlined by the JVM.
A method call takes about 10 nanoseconds (5 to 20 nanos.)  

If you are very good, a microservice takes 10ms, if you are very good.
On top of this call, you must seralize and deser the message on
and off the wire, which I explore in [Message Formats](./2023-09-22-message-formats.md).

10us --> 10ms = 1,000,000 times slower.

Congradulations, you've slowed your code down a million times!

Wait, why is this author carrying on about speed, in a conversation
about scale?  Hmmm... left for the reader.

Hint:  You need to make up for going a million times slower, in the
scale argument.  You will not make up this time, the costs.

### Agility

Amazon incredibly say Agility is a reason to use microservices.
We will simply point this out as laughable and move on.
There is nothing less agile than taking a local method call
and making it a distributed system.

With microservices you introduce schema evolution problems, along with
the rollback problems previously discussed. 

Those schema evelotion problems are not compile time anymore,
(unless you are also committing to using gRPC.)  
This makes you less agile.

### Technilogical freedom

Nothing makes for a better "Distributed ball of mud."
than microservices, and "technoligical freedom."

We can rename the counterarument "too much freedom".
You want a standard set of tools as much as possible, across your stack.

If someone needs a new library, remind them you are there
to deliver customer value, not to play with the latest fad tech.

If they want to propose it as the new tech standard, then let 
everyone move to it, not only them alone, leaving everything
else behind.

### The end of the fad:  Goodbye microservices!

Like every fad, microservices have run their course.
When they first arrived, many of us scoffed.
In sum total, they have led to more mistakes than benefits.

People were already splitting up remote calls as far
back as CORBA.  But no one needed to name them
microservices, or make false claims that they improved developer velocity.

In those days, we split out macro-services when we needed it, 
because they could not run on a single machine.  

These days, needing more than one machine is increasingly rare;
particularly if you already have a load balancer in front of your fleet,
and when you can simply scale horizontally, run the
same code on many boxes.  With Redis and sharding, is it not enough?
Really?

When your developers tell you to use Microservices, ask them
what are the alternatives, and let them use the alternative of
Modular Monolith or Modular Big Service.

Microservices are a silly fad - don't fall victim.
