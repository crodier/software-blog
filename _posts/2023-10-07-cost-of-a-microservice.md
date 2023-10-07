# 60% of your code is deployment

Most of your code is deployment and monitoring; operational excellence features.  Why is this?

## **"You are only as good as your worst microservice."**

What does this mean?  Every microservice you author needs to be hardened.  If any one of them stops
responding you experience a customer facing outage.  Customers won't stick around to use a flaky system
as they expect always on service from any software system.  How do you combat this risk of outages
in any software environment?  It takes a tremendous amount of investment to do this reasonably well.
This effort is rarely cataloged.  In theory the cloud or a large software team makes it easier with
economies of scale but this has not been the case.  Technology has reached a point in the curve
where it has become more expensive than ever to run a high uptime service with a massive skill set needed
to do it correctly.  This typically requires senior engineering time, which is better spent on the 
business facing customer needs, and is the scarcest resource.  Each service comes with a 60% overhead of 
operational excellence.  This cost overhead number may be as low as 40%; however, 60% is a fine number to keep in your head
when you are designing a distributed system.  This will prevent you from splitting up your system 
into microservices which you don't need, you don't want, **and you can't afford.**

### Why programmers favor microservices?  Why do we pay this heavy microservices tax?

Any MBA, or account would tell you that you should not spend 60% more when you can get the same
outcome for half.  The adoption of microservices for most programmers is akin to socialism for programmers.
Where did they come from, and why do programmers often make them today, when they carry this overhead?

#### Be like Amazon

Microservice are allegedly invented or were adopted heavily at Amazon.  But Amazon first started and
was highly **successful with a monolith** after which they transitioned to microservices due to 
their incredible size.  If you want to be like Amazon; then this is exactly what you should do - build
a highly successful and inexpensive monolith.  Once you are a success you can examine the cost/benefits
of breaking it up into microservices - but not before you are a big success.  Some programmers are 
reading this and thinking I am blaspheming as I write this.  Even the naive interpretation of 
the statement *start monolithic* is not false; but do not interpret it as naive.  When you build the 
monolith, use interfaces liberally to separate responsibility; and use **libraries** which provide these
interfaces to the monolith.  Version those libraries and let them contribute to one overall code base.
Using this simple approach you can gain 99% of any benefit of microservices at 1% of the cost.  How is this
accomplished?  Through interface driven design; the same interface driven design you would use when 
designing the microservices.  Treat the internal components of the system as internal microservices; define 
boundaries and separate the code into libraries behind interfaces, and into modules of a larger project
which make those libraries.  This allows developers to iterate quickly on the libraries, in the same
exact manner they would iterate on microservices.  

It is true that this so-called monolith of libraries project (**mono-library-service**)
must be released as one unit.  If your programmers have unit tested and generally tested properly, 
meaning the interfaces perform as expected, then the assembly of the libraries into a system is a
non-event, and the code can be shipped at least as quickly as any microservices stack.

The benefits of libraries go beyond here; way beyond the immediate observable cost of not running the microservice.

You actually save 5x by using this approach.  Here is why.

For each service you build, you also need to build at least
*FIVE* more environments to deploy any service:
1. Production
1. One-Box / Pre-Prod - a slow rollout area for a portion of traffic
1. QA - where you test (assuming you want to review your system before production)
1. Dev - where you take a look before you push to QA
1. Local - **The most important of all** a workstation where a developer will run the system.

Now you begin to understand - with **5 working deployments**, the cost of a microservice is not 5% or 10%.
The ability to run five working copies of your system is where the real cost of microservices arrives.

With 5 environments to maintain, we can begin to appreciate the accuracy of a 60% number.

If there is even a 10% tax on deploying and running one copy of the system (which is a low figure),
this number quickly increases to a more than 50% tax per service.

### Note on avoiding hygiene

If you believe you do not need the five environments where your developers can be productive, we would need
to write an additional article on the enormous cost of having an un-hygienic software practice.  This is a 
situation where people have given up on being able to test before perhaps QA or Prod in any real way.
We can say that at scale, the pace of development is 10-20 times slower than running 
in a hygienic software environment.  An untestable system quickly degrades into one where you can barely 
change anything; any major changes which should take a few months can take a **few years**.  Now you are
left with whatever software you got, running at an enormous cost, and almost impossible to repair or
alter in a meaningful way.  You are unable to meet changing business demands, and costs explode to
make even minor changes.  

Your competition can then eat your lunch as you watch time passing with rising costs, helplessly.
This is akin to a certain kind of software executive, water torture.  It can feel like 
a productivity prison, created by microservices.  You may find yourself re-watching "The Great Escape"
and think about digging tunnels under the microservice fences.

### Programmer mindset

Programmers have tremendous power in organizations.  Executives can't survive without some strong ones.
Given the explosion and continued strength of the software industry there has been an approach of
keep the programmers happy as "we don't want to lose them."  Beyond this, software executives today
don't understand the economics of code and producing code.  They would have needed to spend
a career in software to appreciate these cost benefit concerns; not the same skill set and with
a small union of overlapping skills.  Therefore, you are typically faced with a group of software
engineers who may never have reflected on the cost benefit of code from an ownership level; only from
their part of the puzzle.  These programmers will always favor microservices.  Always.  Why would 
they always favor microservices given this enormous cost?  It makes no sense; yet, it happens repeatedly.

#### Lack of understanding of the cost

Software costs are notoriously difficult to estimate.  It involves such a human element in the design 
and coding and across various pieces of a team ranging from UX to backend and to data; to name a few,
and is usually a combination of these people and skills as very few developers are expert in all of these.

Most developers have never started or built their own successful software business; they are working
in a corporation with other developers and lack the "macro view" necessary to make these kinds of
cost benefit decisions.  This lack of perspective is coupled with the industry having a fad
where microservices are 1) cool and 2) practiced by the big shops 3) more fun to work on due to isolation.
These factors lead to the incorrect adoption of microservices at the wrong time in a business evolution, and for the wrong reasons.

### What are these costs? (x5 for each environment)

Earlier we mentioned the importance in software lifecycle terms, of having FIVE software environments.

It may seem that each of these are merely a copy of the same system running somewhere else, but 
this is not nearly the case in practice.

The key areas which are additional costs to every microservice:
1.  Maintenance and patching (upgrades to system, language, frameworks)
1.  Deployment configuration
1.  Monitoring / Metrics
1.  Alarms
1.  Logging and log rotation
1.  Security and Permissions (can't get hacked)
1.  Cloud infra; provisioning, costs of cloud
1.  Documentation (the often neglected docs.)

In summary when you take the costs of running a service *properly*, 
because you are only as good as your worst microservice, you experience cost
explosion and experience the costs as low velocity software teams; the time 
it takes to implement features (as time is money.)  

Your team are paid by the day and software takes a long time.
Given these realities you do not want to invent a cost-explosion for yourself, yet most shops today do this,
and then wonder "why did we do this, this doesn't seem to be working."  

This is most experienced by the business (CEO, investors) when they become dissatisfied with progress.
Your system takes a virtual eternity to update due to the software design and the microservices fad.
But without deep foundations in software costs these become extremely hard to measure and would require
and entire team of software velocity measurement experts; observing the software team, against another
software team.  As you will always lack this "control experiment", you will never know.  As a
*journeyman* infrastructure generalist, you will need to take my word for it; but you can observe this
for yourself, if only via anecdotal evidence which confirms this hypothesis.  Here is where
the *data driven* folks would ask for proof that microservices are more expensive.  This is the wrong
question to ask.  Microservices are clearly more expensive; 2 > 1, by mere counting, if you want data.
The proof which is necessary to justify doing something 2x, and mandating your business incur this 
cost, is to PROVE THAT YOU NEED THIS COST.

This document gives you the outline; how much more effective must we be?  **You must be 60% more effective
with the microservices split, to justify using microservices over libraries.**  You can come up with
your own number if you wish, but if it is < 60%, you are probably lying to yourself, or having a 
programmer lie to you because they prefer microservices.  Those programmers don't care about the costs though,
as they are not incentivized properly to cut costs.  In fact, higher complexity _favors_ the programmers.  

There is a clear reason this happens, repeatedly, 
and this is an obvious outcome of corporate interaction... get ready here it is....

## The argument: Microservices deploy quicker

**The argument:**  A junior programmer will tell you there is **obvious** cost savings to microservices.  We don't need to 
wait for team Y to deploy and we are team X.  This is a clear savings.  There are some costs, but 
nothing in comparison to the cost of waiting a week, or a month, to ship my software.  This is frustrating and
we don't like it, and for this reason, we need to do microservices (or micro-frontends!)  Micro-front ends
will have another dedicated article debunking the myths there; however, the discussion is nearly identical.

Armed with 1) this **obvious** deploy time argument, 
and 2) wanting to keep our programmers happy (we do not want unhappy programmers, they may quit.) and 
also knowing 3) Big dot-coms build microservices, we can see there are now **three strong arguments**
for microservices.  Only an idiot would go against this enormous evidence in favor of microservices; 
and in addition, many programmers have said "we prefer microservices."  We don't need to analyze the
costs because we know this has worked for the big shops and it will work for us.  


This ends the surface level arguments.  These are all 
surface level arguments without any understanding or questioning of **Why We Care** about any of these arguments,
or the opposite approach.

But, you are busy, and don't want to go against engineering.  Going against the fad obsessed junior programmer
requires effort, and causes conflict.  Will you be rewarded by saving the company 2x in software velocity,
by taking a very public stand against microservices or microfrontends?  You will not.

Conflict is not rewarded in corporations.  Software engineers are a sensitive bunch, usually introverts
who don't mind spending long days with no human interaction and mostly on a computer.  Upsetting the 
programmers can cause people to quit, and in an environment where tech talent is hard to find,
why would anyone disagree with a star, enthusiastic, junior programmer?  This will not make a manager
look good in their 360 review; and they may not even understand the arguments involved.  Only with
deep technical chops can one debunk "the need for microservices."  It is such a nuanced and deep topic
as to the beneifts, and there are clearly surface arguments (without any depth), in favor of microservices.

This and the herd mentality (don't upset the herd) will lead to microservices being adopted.

Finally, those arguing for microservices may be *passionte engineers.*  They may even be the brightest
engineers on the team.  

The question is, why would the programmers want the microservices; why would anyone be passionite
about something which costs 60% more with no perceivable value?

#### Two-pizza teams

As an aside, the microservices also favors "going fast" and working in small teams.  If Amazon Retail
was successful this way, and this is culturally favored, why should all teams not favor this dogma?
This is yet another argument (at Amazon), as to, the question **"why microservices?"**

It could be said that this approach to employ the dogma from the success of Retail across all business lines
is leading to very demise of Amazon; the adoption of a "best practice", without though any data to support 
the belief that microservces are better *for the task at hand.*  This is where **cost data** would win the argument
but there is no control experiment, therefore, there is no compartive cost data; and the dogma is followed.

## Why the programmers prefer Microservices

Programmers are introverts.  And code and the quality of code is relative to the task at hand.
Most code can be written and solve a small problem multiple ways.  In React, you could be using
web-hooks, or you could prefer React before WebHooks.  In Java you could be using Lambda expressions with streams;
or you might prefer using a collection with a for loop.  This comes down to mostly taste.  Except when it 
does not come down to taste, and is actually important.

More important than taste is time to market.  When pressed with a deadline; if your job is on the line,
any working code will do.  I have been in a situation where if we did not release on a current day
our team was going to be let go, in a horrific hedge fund where another team had accused us of being
incompetent.  After making our deadline, the other team was let go, which sounds and is unfortunate.
But they did indeed say we were idiots and that we would never finish; much less in six weeks, but we did.
While this is an extreme circumstance, any business relies on the programmers being efficient with their 
time.  Programmers are one of the most scare resources for any business.  It takes the large majority
of humans ten years professionally to become a strong programmer, and another ten to become an expert.
There are not enough working 20+ year hands-on programmers.  Most move into management long before
the 20 year mark.  This is another symptom of the problem of programming style; and problem of the cost-value 
of writing code.  Code can always be better, and a system can always be better.  Any code can 
be optimized further for latency and is typically _sub-optimal_ latency wise.

The question which should be asked before shipping code, is this code good enough fot the task at hand.  This
is the correct question to ask yourself.  The interfaces are more important than the implementation,
and the implementation is important but it rarely is perfect and it rarely needs to be perfect.

Given the above situation with deadlines, and code perfection, we now come to the crux of 
one true benefit of microservices; and it is not deployment frequency.

### The real benefit of microservices:  judgment calls

The benefit of microservices is the situational awareness and the cost-benefit analysis in the 
time spent on the code and the relative risk taken in the decisions.  

When you work in a microservices environment, the programmers can make those judgement calls in 
isolation.  Libraries also afford a similar independence though, at 1/10th the cost of the microservices.

When you move to a shared code base there is an opportunity for **organizational dysfunction and 
distress.**  Programmers don't like this and leads to low job satisfaction scores, which 
have become severely overvalued in business today.  How does this dysfunction arise?

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

Now you have two groups of people who are largely disconnected, with competing incentives.
It is often the case they may have different management teams and in the worst case
different entire organizations.  If they can't agree it leads to friction in an organization.
There are two ways to view friction.  A wise man said to me if there is no friction
then you are doing it wrong.  Friction is typically avoided in business.  It 
seems like smart people who are doing an intellectual activity should not have friction
in business.  But this is not the case, there is friction, and here is where it will occur
when you do not use libraries or mirco-services.  Both libraries and microservices offer the reduced friction
but the libraries are thrown out the door for "faster deployment", with a total disregard
for the costs of microservices.

The programmers know about the friction, by repetition and experience; 
and they want to avoid the friction at all costs.  

The last time they felt the friction it was
probably a mean senior programmer like me, doing a code review, telling them how 
they could improve their code.  They wanted to go rock-climbing or whatever 
else and had already worked 50 hours to get the code working and tested.  Now 
here I come asking them to spend another 20 rewriting the code in a code review.
But the code worked before, and will be functionally equivalent; is it worth their time, their life?

To the so-called senior programmer doing the review, it is worth the time.  But 
in this code review it is rarely considered; is the time spent improving this code
or even having this discussion, worth it?  Does the business benefit, or are 
we merely doing intellectual wanking?  Most of it is intellectual wanking and
the code will not live long enough for it to matter how the internals are written
or if the log levels are ar the right warn or error level.  But these details
will come up in a code review and be debated.  One of my favorite Java
code review practices is around access modifiers on variables, and if a public
variable should be made private (and final.)  You can spend a year coding python and
never think about access modifiers because they don't exist (for better or worse), 
and then arrive in a Java code review and spend ten minutes discussing access
modifiers.  Was it a wasted ten minutes of your life?  Most likely it was wasted, yes.

If no one uses a variable maliciously or mistakenly than it is more work
to make it private (and final); but this best practice is anyway considered _de rigeur_ in Java.

But the code only needs to be good enough; the code is never perfect.  There are more 
important things to work on, or not work on.

### The joy of microservice programming

To avoid these "how to code" and "what is art" debates, introverted programmers
will flee to the land of microservices, where we do not need to talk to each-other.

For the programmer working on the microservice it is a kind of nirvana; a drug-like experience.
You are back to being in college coding alone on a project, and the only grade is "does it work."
You get to experience the creative joy of authoring something yourself unencumbered.
You can use all the latest fads of coding, and no one will realize are trying out new
techniques which allow you to experiment with new tech; which may or may not be good for business.

The cost is incredible; 60% more than what the project should have cost.  Most of 
this cost is maintenance, care and feeding of your microservice, which you will not 
experience after you launch it.  With your improved skills in microservices, you can
move on to the next shop where they are struggling to comprehend this amazing discovery.
But you with your microservice experience can now shed the light, and show the management
team that while they have 60% less money in their pocket, the happiness is worth it.

The programmers are never *billed back* for this additional 60% cost; the toilet in your 
newly remodeled bathroom may cease to flush, all you can do is pay more to maintain it.

Once the toilet is in place, you can't survive without one.

You now need even more,the software plumbing and the programmers who minted it for you.
And you need more of them to do it.

Instead of needing one programmer, you now need a team of programmers to maintain the microservice.

You may need a DevOps team to manage the microservices in addition to the team you are on.
You probably invented the need for a _latency investigation team._  This is how you 
know you have been taken to the cleaners by your software contractors.  
They are happy, but you are not getting what you wanted out of the investment.

All of these problems exist in Big Tech, and everywhere, due to the microservices myth,
and misunderstanding

Management can sense, something seems wrong here,
as our calls can take up to 10 seconds to do some trivial operation, and our 
team sizes have ballooned.  Could it be we have adopted the entirely wrong approach 
to programming?  It would be challenging, and an angry mob would likely run
any manager out of town if they were to stand up and say, we have failed with our 
microservices approach to coding.  It would be going against the existing success
which was achieved; how can you be critical when you didn't write this software they may say?

It can be difficult to go against the grain; and would not lead to a good review.

I can be critical because I have a history of making careful
cost-benefit judgements along the way, like these.  
Good friends of mine in management roles, who seek me out to be successful in cost-constrained 
and high velocity software environments.

I take joy in the craft, but I am in software to be successful first and foremost 
fo the business and teams I am on.

# Conclusion

When faced with the decision of should this be one service or two, or many, the 
most important question is cost.  To achieve savings you must have better than 60% 
cost savings when running a microservices approach, per microservice created.

There are times when you must scale one component to a degree where it must be isolated
and turned into a microservice.  These scalability challenges are rare but they do exist.
In these rare cases where a microservice is manated, it must be employed; **despite the enormous cost.**

You can say to yourself, this is unfortunate but we must employ microservices here.  There is indeed
a time and a place.  But unless you are compelled to, there is rarely a case when you should use
microservices, with the enormous 60% tax.  When you do, the tradeoff should be carefully examined
and justified the business owners, should ask repeatedly, are you sure?

## Are you sure?

Are you sure you understand, the cost-benefits?  And are you sure we need to take on the enormous cost?

When you do go down this road, more important context:
1. [Domain Mode](./2023-09-17-domain-model-importance.md)
1. [Message formats](./2023-09-22-message-formats.md)

If those two decisions are not done nearly perfectly, you will have undone any benefit
you may have achieved from microservices, and created a latency or productivity nightmare.

_Be warned!_

# Appendix

## The right ship for the job:  How big shops wage war

Another note on cost; you can think of Microservices as the aircraft carrier of the software navy.
They are used by major armies to win naval software battles.  They are impressive engineering feats
when done correctly, these aircraft carriers; powering major e-commerce and search engines.  
When a programmer is faced with what to build, they will think, we should be like the big guys,
and build an aircraft carrier.  Put this way, we can easily observe the problems with this statement.
First of all you need to be naval expert to design and build an aircraft carrier.  The big shops
have this kind of expertise; decades of naval ship building experience, which you likely do not have.
Second, the big shops need an aircraft carrier; they need to launch and land airplanes off the deck.  This
is an important engineering feat.  They need to do this, to compete with the other **nation states** who
are in this particular line of business.  Before you adopt an aircraft carrier approach you need
to ask yourself if this is a *nation state* size problem.  Will you be going to war in the Pacific
against another super-power?  If you are **Amazon Retail** (www.amazon.com); the answer is yes.  You are 
going to war against super-powers like Target and Walmart.  You should design to have the greatest 
weapons you can bring to the fight and spend enormous investments in this technology.  If you are 
going to complete with Google in search you very likely need microservices.  In any other
business environment the likelihood is quite low you are doing battle on this level, and you should
avoid the engineering costs associated with it.  It is true, aircraft carriers are cool; everyone
has seen Top-Gun and wants to be Tom Cruise.  But this is not the movies, and you are in business.


## References

#### Amazon Prime move to monolith

### Web-scale YouTube Video

