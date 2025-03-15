# Enterprise Software Anti-Patterns

Here are the common **anti-patterns** (fads) in enterprise
software in March 2025.  

We explore the reasons, along with the solutions which should be preferred.


| Anti-pattern   | Reason                    | Better solution                | Why                 |
|----------------|---------------------------|--------------------------------|---------------------|
| Microservices  | Cost, Latency, Complexity | Modular Monolith               | 1,000,000x latency  | 
| Lambdas        | No state, remote calls    | Modular Monolith               | Cold-boot and state |
| Microfrontends | UX paramount, bloat       | Monorepo + Bit.dev / Ripple CI | User experience     |
| Kafka          | At-least-once, Rebalances | gRPC (or Solace)               | Wrong tool          |
| Spring XML     | XML based runtime language | Spring Boot / Dagger / Guice   | Obscure XML coding  |


Note: developers will always be dissatisfied.  Developer satisfaction
can be a secondary goal, but never at the cost of a bad UX for your business customers.
Working on a bad product or failed business, will be much more dissatisfying, for your 
engineering community.

Coding is a challenging human activity, painstaking labor.  

Deployment independence may be
solved, and developers will find something else to complain about.  So let them complain about
deployment independence, and have
a highly successful business; bring in free lunch with the extra money you make
by not favoring deployment independence , maybe they will stop complaining about
having to wait two weeks to deploy.  Note this is not real time to them; this is only
time waiting for the build to be completed with the features they did not contribute.
Deployment independence can be achineved many ways, and "micro stuff" is the wrong way to achieve it.

Its the first one people reach for, and the worst alternative to deployment independene,
which was anyway not somethign you cared about, and was a false idol to begin with.

## Microservices ANTI-PATTERN

Microservices are an anti-pattern of software development.  Did you know that Amazon Prime Video
moved from a microservices arch into a Monolith, with amazing results.  This is happening
everywhere in enterprise software, where first principles of software develompent are
used.  "Developer velocity" is not a feature for your business, and libraries have 
been used since time eternal to decompose software systems.  

As discussed in
[Cost of Microservices](./2023-10-07-cost-of-a-microservice.md) and [Microservice Myths](./2025-03-13-microservices-myths.md)
a call in memory is 1,000,000 times faster than a network call.  
You avoid this incredible penalty at all costs.  AuthN/AuthZ is a good service to build.
It is however, not a microservice; it is a service *tier*. (or a Macroservice.)
Best article on Modular Monoliths is here [Modular Monolith](https://www.milanjovanovic.tech/blog/what-is-a-modular-monolith).

## Lambdas (AWS Lambda) anti-pattern

AWS Lambdas have a severe cold-boot penalty.  It is getting better.  You don't
want the enormous complexity of cold-boot jitter, and the expertise needed to 
work around this problem.  Anything interesting requires memory.  AWS will tell 
you to use ElastiCache and Redis.  This is a now a remote system and now
instead of making a mehod call, you need protobufs, Redis, and maybe Terraform.
What was once loading a hash map of zip codes into memory is now a 15 person 
excercise, with a DevOps team, and the pipelines to match.  The costs have exploded.
This is in no way worth doing Lambdas.  Everything you do will need some state,
and then incur this incredible overhead to change your programming model for this fad.

## Microfrontends Anti-Pattern (yes, itself)

Your dev team says they want to use the latest version of React, but no one has
time to upgrade any of the old components.  Now you are stuck, and you don't
want to use many multiple versions of react.  Once you move to microfrontends,
you are moving towards a world where you load three or more versions of React,
along with the assocaited library explosion.  You may think you will police your
developers; but now you need everyone to know the rules, and carefully police
what libraries are being shipped to the front end.  You won't do this, as
you don't have the budget, and no one gets any promotion for doing this
thankless work, of "JS library policing" the other developers.  Next you
end up with a bloated front end, which is crappy UX wise.  Your entire job 
was to make a UX which was snappy and fun to use, and now you've made a pile
of microfrontend crap, which is slow to load and no one wants to use.

If you think latency of the front end is not a big deal, go open your Gmail
and remember every time you touch your phone, you wish it were faster, as
the seconds of your life tick away waiting for network calls and JS libraries
to load.  All your customers are the same as you, and you didn't realize
how much you dislike latency.  You dislike it more than anything.

You can make latency a stated goal of your UX, and still have problems.
With Microfrontends, you pretned that you can fix latency later (you can't),
and then you don't fix it, and have a slow system no one likes.  

Everyone wants to use your system on their Meta glasses, but you have
too many javascript libraries to load.  You've failed, and need to start over.
Millions of person hours invested, in what will always be a slow crappy UX,
without starting over.  "At least we have the lessons learned."

## Kafka as an inter-service bus Anti-Pattern

Kafka is good for pushing your click logs from your big website, and then 
reading from it and inserting it into your big data system or database to 
be reviewed and mined to get a few more ad impressions.  It is the worst
product ever as an inter-service message bus.

Kafka takes ten seconds to connect to, when you start your app.  You will
be doing this many hundreds of times, and waiting the ten seconds.

More importantly, this same penalty is incurred when Kafka Rebalances.
Kafka can rebalance at any time.  When a new prodcuer joins, it could decide
to rebalance.  Nothing bad has happened; Kafka wants to help you, and it
thinks it is re-distributing the load of your non-real time, not customer facing,
log parsing ML pipeline.  Kakfa does not know that your customer is waiting for that
message to go through.  The message takes 10 seconds, and you have severe jitter in 
your app.  This goes back to the discussion of latency; however, it is the problem
of enormous jitter.  The more brokers you have, the more rebalances you will observe
in prod.  Kafka for this reason is wildly inappropriate as a message bus between
user facing services.  The only thing it is good for is publishing logs.  Don't
introduce Kafka as the most complicated and wonky message bus.  You then need
a team of Kafka tuning experts.  At one major dot com, principal engineers
would seek me out, to fix their Kakfa problems.  They had teams of fifteen engineers
dedicated to keeping Kafka running smoothly.  Now there is Managed Kafka, but 
it still stuffers from rebalancing issues.  Don't bother, it is the wrong tool
for the job.  You are already writing your log messages to disk and keeping those logs,
and you don't need yet another log to replay them.  If you want Kafka as a backup,
then you can send the messages between services using gRPC as you should do, and then
spam them async to Kafka on a background thread, should you want "the tape" to be
published to Kafka as well.  Instead of this, just log, and have a log reader
push the messages to Kafka.  Facebook does this with [Scribe](./https://engineering.fb.com/2019/10/07/core-infra/scribe/)
, decentralized.  The most intersting part to me is the usage of the side car.
After a quick review it seems [syslog ng](https://www.syslog-ng.com/) may 
be a good one to use, if you truly need a backup copy of every message for replay.
Do this out of band.  The jitter on Kakfa is incredible, and Kafka will add
a 300ms p90 penalty to every interation.  The standard response time
targeted in my development is 200ms.  If you don't target this, your p90 will 
be above 1000ms, and your app will feel slow (and crappy), for users.  
You may have the richest UX on the planet.  If it is sluggish, no one cares.
As an Interactive Brokers, desktop user, I'm a prime example.  All
traders I have worked with, and all humans, have followed this standard.
We dislike waiting and spend considerable portions of our lives now, 
waiting for things.

[100ms](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://www.nngroup.com/articles/response-times-3-important-limits/&ved=2ahUKEwjyk9m49oyMAxXPF1kFHfvMEWAQFnoECDgQAQ&usg=AOvVaw0fjOHQ6Kt6juwh3kEc08Y4)
is the target for human interaction.
[Google targets 200ms max.](https://developers.google.com/speed/docs/insights/Server)

## Spring XML

Spring XML was my first ever anti-pattern idea in the enterprise, and
this grew, was mentioned in the industry.  At the time
I had begun saying Spring itself was an anti-pattern.

When used incorrectly, spring is an anti-pattern.  If there is only one
implementation of a class, there is no need for an interface or to abstract it.
The reason you use interfaces is for mocking and testing.  People often don't 
do this, and only ship with the interfaces.  But there is a reafactoring in 
IntelliJ "Extract interface" which can do this for you, later when you need it
(not to mention ChatGPT.)  

In the end Spring Boot is incredibly useful.  The SpringXML syntax was an
is horrible.  "contructor-arg" is the only
note I need to leave here.  If you have typed "constructor-arg", then you know.










