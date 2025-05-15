---
title: "Cost (trade-offs) of Microservices."
date: 2023-10-07
layout: post
published: false
---

# 60% tax per microservice; where is the value?

Before reading this article, it is useful to review the typical 
view of a reasonably large microservces deployment from a 
product perspective:

[Microservices (KRAZAM) ](https://youtu.be/y8OnoxKotPQ?si=FQbN1gwR8M9rvvNj)

By splitting a service into microservices you increase your project cost by 60%.

Think carefully before you 
take something done simply in one service and split it into two
or more microservices.  You pay a 60% or higher cost to do 
the work of one service in two microservices.

### Why do these problems occur with microservies?

1.  A large tax for deployment and monitoring; operational excellence features.
2.  Five environments are necessary to run an effective software practice
3.  Every developer must be able to spin up a development environment, and work across the entire system!
3.  Complexity and latency increase considerably when moving to microservices

### What else can we do?

Prefer libraries, and use the "service locator" pattern
as a insurance for a future time when you may need microservices.

Do not blindly tax your organization 60%.

Start without microservices.  

Only pay the microservices tax when it becomes a clear and obvious necessity.  

### Steps before engaging on Microservices

Be sure to first document and review:
1) Cost benefits of the microservice approach; what is gained?
2) The cost difference of assembling libraries into a service; overhead costs?

Libraries are a tried and true approach with well-known benefits.
Prefer to rely solely on libraries and dependency injection 
until you are an expert
user of libraries, and CI/CD with those libraries.  

Only then should you consider microservices.

## **"You are only as good as your worst microservice."**

What does this mean?

Every microservice you author needs to be completely "hardened".  
If any microservice stops
 responding, you experience a customer facing outage.  

It takes a tremendous amount of investment to handle high uptime reasonably well.
This effort needs to be cataloged before moving into microservces.

In theory, the cloud or a large software team makes it easier with
economies of scale; however, this has not been the case.  

Technology has reached a point in the curve
where it has become more expensive than ever to run a high uptime service with a massive skill set needed
to do it correctly.  High uptime services require senior engineering time.
Senior engineering time is scarce and expensive.

Next we explore why each microservice comes with a 60% overhead of 
operational excellence.  This cost overhead number may be as low as 40%; however 60% is a better reference number
to use as a starting metric.

This metric is a useful warning label.

### Why do programmers tend to favor microservices? 

We should avoid an additional 60% more when you can get the same
outcome for half.  Despite the cost, microservices are adopted 
as a form of idealism.

Where did they come from, and why do programmers often make them today, when they carry an enormous overhead?

#### Be like Amazon

Microservices were adopted heavily at Amazon.  
But Amazon first started and
was highly successful with a monolith, after which they transitioned to microservices due to 
their incredible size.  If you want to be like Amazon 
then perhaps follow their footsteps, by building
an inexpensive monolith.

Once you are a success, you can examine the cost/benefits
of breaking it up into microservices — but not before you 
are a big success.  

When building the monolith, *use interfaces liberally* 
to separate responsibility.

Next, use libraries, along with depencey injection and dynamic loading to provide these
interfaces to the monolith.

The libraries are then version controlled.

Version those libraries and let them contribute to one overall code base.
Using this simple approach, you can gain 99% of any benefit of microservices at 1% of the cost.  

How is this achieved?  Through interface driven design; the same interface driven design you would use when 
designing the microservices.  Treat the internal components of the system as internal microservices; define 
boundaries and separate the code into libraries behind interfaces, and into modules of a larger project
which make those libraries.  This allows developers to iterate quickly on the libraries, in the same
exact manner they would iterate on microservices.  

It is true that this so-called monolith of libraries approach
must be released as one unit.  If your programmers have unit tested and generally tested properly, 
meaning the interfaces perform as expected, then the assembly of the libraries into a system is a
non-event.  

The code can be shipped at least as quickly as any microservices stack.

The benefits of libraries go beyond here; way beyond the immediate observable cost of avoiding the microservice.

You can actually increase productivity dramatically using this approach.

### Why is productivity improved?

For each service you build, you also need to build at least
five more environments to deploy any service:
1. Production
1. One-Box / Pre-Prod - a slow rollout area for a portion of traffic
1. QA - where you test (assuming you want to review your system before production)
1. Dev - where you integrate and review, before you push to QA
1. Local - a workstation where a developer will run the system.

With 5 working deployments, 
the cost of a a microservice approach is not 5% or 10% more
than a monolith.

The ability to run five working copies of your system is where the real cost of microservices arrives.

With five environments to maintain, we can begin to appreciate the accuracy of a 60% estimate.

If there is even a 10% tax on deploying 
and running one copy of the system (which is a low figure),
this number quickly increases to a more than 50% tax per service.

### Note on avoiding hygiene

If you believe you do not need the five environments where your developers can be productive, we would need
to write an additional article on the enormous cost of having an un-hygienic software practice.  This is a 
situation where people have given up on being able to test before perhaps QA or Prod in any real way.
We can say that at scale, the pace of development is 10-20 times slower than running 
in a hygienic software environment.  An untestable system quickly degrades into one where you can barely 
change anything; any major changes which should take a few months can take a few years.  

At this pace you may be unable to meet changing business demands, and costs explode to
make even minor changes.  

This is a productivity prison, created by microservices.  

The productivity loss is the subject of the video, which is
not an inaccurate picture of reality 
with a large microserices system.

### Lack of cost analysis

Software costs are notoriously difficult to estimate.  
Software engineering. is a notoriously difficult activity to measure.

A macro view of the impact on software velocity is necessary to evaluate the impact
over a time horizon of quarters or years.


### What are these costs?

Earlier we mentioned the importance in software lifecycle terms, of having five working software environments.

It may seem that each of these are merely a copy of the same system running somewhere else, but 
this is not nearly the case in practice.

The key additional costs to every microservice:
1.  Latency measurement with remote calls
1.  Maintenance and patching (upgrades to system, language, frameworks)
1.  Deployment configuration (and auto-rollbacks etc)
1.  Monitoring / Metrics
1.  Alarms and Canaries
1.  Logging, log rotation, and log search
1.  Security and Permissions
1.  Cloud infra; provisioning, costs of cloud
1.  Documentation

When you review the costs of running a service *properly*,
you experience cost explosion in the form of 
low velocity software teams; the time and people 
it takes to implement features.

The data driven crowd 
would ask for proof that microservices are more expensive.  
This is the wrong
question to ask.  
Microservices are clearly more expensive.  
Two high uptime services to operate in production, are clearly more than one service to run.

If cost is being measured a-prori someone simply need list and reasonably
esitmate the overhead, and why, as I have done above,
before you venture down the microservices path.  

How will you make up for the 60% overhead, and,
what cheaper alternatives exist to accomplish the same goal?

How much more effective must we be?  You must be 60% more effective
with the microservices split, to justify using microservices over libraries.

Other figures can be chosen, but if it is < 60%, you are likely not measuring properly.

## The arguments for Microservices

Strong arguments can be made and are indeed true about microservices; however,
they respresent a fractional value compared to the overall cost, in all
but the largest software deployments.

**Argument 1 - deployment isolation:**  With microservices teams may not need to wait for each other to deploy.
This can be a savings.

**Big dot-coms build microservices**  Coming from large dot-coms
we can observe there is a necessity for microservices.

**Microservices feel better:**
Many programmers on the team may have said "we prefer microservices."

--

Only with deep technical chops can one debunk 
"the need for microservices."  
It is such a nuanced and deep topic
as to the benefits, 
and there are clearly surface arguments which can be quoted
in favor of microservices.


The question is, why would the programmers want the microservices; why would anyone be passionite
about something which costs 60% more with no perceivable additional value?


## Why the programmers prefer Microservices

Programmers are generally introverts.  

The quality of code is relative to the task at hand,
along with business pressure to deliver. 

More important than taste is time to market!  

When pressed with a deadline; if your job is on the line,
any working code will do.  

Programmers are also one of the most scare resources for any business.  

Code can always be better, and a system can always be better.  Any code can 
be optimized further for latency and is typically sub-optimal latency wise.

In business and in general, the question which should be asked before shipping code, is this code good enough fot the task at hand?  This
is the correct question to ask yourself.  

The interfaces are more important than the implementation,
and the implementation is important but it rarely is perfect and it rarely needs to be perfect.

Given the business deadlines, and code perfection, 
we now come to the crux of 
one true benefit of microservices; and it is not deployment frequency.

### The real benefit of microservices:  judgment calls in isolation

The benefit of microservices is the situational awareness and the cost-benefit analysis.
This is where programmers make a relative value judgement on the code and the relative risk taken in the decisions.  

When you work in a microservices environment, programmers make those judgement calls in 
isolation.  Libraries also afford a similar independence though, at 1/10th the cost of the microservices.

When you move to a shared code base there is an opportunity for organizational dysfunction and 
distress.  Programmers don't like this and leads to low job satisfaction scores.  

How does this dysfunction arise?

#### Code reviews, and release cycles

The first major point of dysfunction is in testing and the release cycle; the second one
is in code reviews.  Here is where if the system is not a microservices based system
teams of engineers must work closely together and truly understand the cost-benefit
judgements of the other team, and make compromises.  

One team may be largely responsible for
the uptime of the service and another team for adding a feature to the service. In 
this environment the uptime team is focused on code quality and long term ownership 
and maintenance.  The feature team needs to release asap.  These are two competing priorities.  
How the code looks and how well it performs is largely a judgement decision.

Now you have two groups of people who are largely disconnected, 
but with competing incentives, working on the same system.
It is often the case they may have different management teams and in the worst case
different entire organizations.  If they can't agree it leads to friction in an organization.

The programmers know about the friction, by repetition and experience; 
and they want to avoid the friction.  

Friction comes in the form of code-reviews, design reviews, and interaction
with other groups of programmers with other approaches, other preferences.
With one shared code-base the culture needs to agree on certain approaches.
With microservices the cultural clashes can be avoided to a large degree.

### The joy of microservice programming

For the programmer working on the microservice it is a kind of nirvana.

You get to experience the creative joy of authoring something yourself, maybe unencumbered.

But the cost to achieve this isolatoin can be incredible; 60% more than what the project should have cost.  Most of 
this cost is maintenance, care and feeding of your microservice, which you will not 
experience after you launch it.

As a business you need even more programmers, more software plumbing.
The programmers who know the services arch are the programmers who minted it for you.
And you need more of them to do it.

Instead of needing one programmer, you now need a team of programmers to maintain the microservice.

You may need a DevOps team to manage the microservices in addition to the team you are on.
You probably invented the need for a _latency investigation team._

Programmers will be happy, but a business may be unproductie as a result.

I also take great joy in the software craft.

But I am in software to be successful first and foremost 
for the business and teams I am on.

# Conclusion

When faced with the decision of should this be one service or two, or many, the 
most important question is cost.  To achieve savings you must have better than 60% 
cost savings when running a microservices approach, per microservice created.

There are times when you must scale one component to a degree where it must be isolated
and turned into a microservice.  These scalability challenges are rare but they do exist.
In these rare cases where a microservice is manated, it must be employed *despite the enormous cost.*

You can say to yourself, this is unfortunate but we must employ microservices here.  There is indeed
a time and a place.  But unless you are compelled to, there is rarely a case when you should use
microservices, with the enormous 60% tax.  When you do, the tradeoff should be carefully examined
and justified the business owners, should ask repeatedly, are you sure?

## Are you sure?

Are you sure you understand, the cost-benefits?  And are you sure we need to take on the enormous cost?

When approaching microservices, two other articles may help:

1. [Domain Model](./2023-09-17-domain-model-importance.md)
1. [Message formats](./2023-09-22-message-formats.md)

_Be warned!_

# Appendix

## References

#### Amazon Prime move to monolith

https://blog.devgenius.io/amazon-prime-video-reduced-costs-by-90-by-ditching-microservices-a9f80591f96a?gi=721c00f381db
