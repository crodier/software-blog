I want to chronicle the largest failed projects
I've seen in software, as there are shared qualities.
The number one shared quality in failed or poor
software investments is lack of detailed analysis
of the problem space, the alternative approaches, 
and the cost-benefits of those approaches.

You would be shocked, to know that in software,
$100,000,000 software projects which are total failures,
are minted all the time.  Probably more
in software than anywhere in history, are the size
of failed businesses.  Second to this is the
amount of investment in failed "in-house" systems, inside major 
businesses.

I can save you from green-lighting your next $100M 
failed software project, having participated in three others.

My fee is only 10%:  $10M (an incredible bargain!)

### Distributed Spring XML Java RMI Global Data Cache

The first massive failed project I joined was at
an investment bank, where a decision was made to author
a distributed cache using Java RMI.  

The hypothesis
was that because RAM usage of one system was exceeding 4GB, 
that in this 32 bit software world, 
we needed a distributed cache to hold the data.

In 2008, while most machines were 32bit, with only 4GB RAM
addressable by the JVM; 64 bit computing existed and was taking
hold.  Perhaps at the start of the project in 2004, this was
not the case, but it was certainly the case in 2008 that 64 bit
machines existed as commidity hardware.  

The project needed to be re-evaluted
in this new tech landscape, as the fundamental assumption 
had changed.  Yet, a large team of about 15 developers 
and multiple managers, worked full-time on this custom,
distributed RMI cache.  

This was one major obvious flawed / missing analysis, was that 4GB was the limit, and that 64
bit computing was not a commodity or was far away from being one,
but this was never the case.

### Passionate and smart junior developer

Passionate developers are difficult to disagree with.
Managers are evaluated on their teams' happiness, and 
they are often afraid "my develpers will quit," and then
seem to be strong-armed into projects which should not be
green-lighted.  The most difficult ones are the brilliant
and passionate ones, in their early years.  They have 
been brilliant their entire life and never run into failure,
many of them.  This makes them very confident. 

Managers assume 
their developers are making good judgment decisions.

Developers who are very capable of delivering **a basic
business system**, are *not* the developers who should be 
**inventing software infrastructure.**  

Business developers
are not software infrastructure experts (this includes me.)
I've spent most of my career using databases, and big-data
and other infrastructures, combining them into business systems.
The difference between me and other senior developers, is
I know the incredible gap in understanding I have, and what
it takes to author an effective infrastructure myself.

Authoring an infrastructure is a leap left to infrastructure
gods.  If you can't talk forever about the beauty of a B+
tree, Bloom filters, LSM-tree, Bitmap indexes and many
others (I can barely), *inventing* big infrastructure is
far more challenging than you can imagine.  When I grow up
 someday, I'd like to author the cloud, but until then,
I'm a business systems engineer - a user of infrastructure,
not an inventor of one.  If no one on your team has invented
infrastructure before, or is a noted infrastructure guru,
then they are almost certainly, 
not qualified to invent a new infrastructure for you.  

This is another quality of the biggest failed sotware projects
I've been a part of - is where business systems engineers,
felt they were qualitifed to invent and run, a software
infrastructure.  

Inventing software *frameworks* is a similar problem.
Business developers should be exclusively users of these systems.
Authoring frameworks is wading into the deep end, and
if you have not been there before, and are not asked to author
one but are offering to author one, be warned - there be dragons!

The dragons - are you realizing failure modes you didn't know
existed when you started, and you figure them out at the
end of the project.  This is **"you don't know what you don't know"**
about software infrastructure.  

I'm (usually) in the class of 
"I know what I don't know - and am not pretending."  It turns 
out I'm in the minority, because it seems like engineers who 
are using a database or a framework, seem to make a leap 
of faith that they can author one, having used one.  

This is not the case.  It is a good test:  has your
team worked on an important framework before, 
perhaps a mildy successful one
like Angular, or a core React team member, or Vue, before
**as a contributor**?   If not, do they know what they are up against?

Software infrastructure is 10x more challenging than frameworks.
Particularly data infrastructure.  Transactions and
latency, are incredibly unforgiving things to deal with,
and they will show you what you don't know.
Data integrity under failure,
and distributed latency p100, will crush your soul,
and they have crushed mine many times.  Like Yoda says
*be afraid, be very afraid.*

### Bad hiking analogy but fitting

Back to this story, the team had no one who had ever 
authored a cache, and had no particular experience in caches.

A side-bar is that this is like a local hiker saying,
I plan to hike Everest, without a guide.  

They are fundamentally the same activity, **merely** different
difficulty hikes, but just another hike. (?)

Hiking Everest alone, is like trying to invent software
infrastructure, as a business systems engineer.

As we know, hiking Everest, requires a Sherpa or a team
who have hiked Everest before.  You would not want
to be the first solo hiker of Everest, to do it alone.
Someone indeed did it.  Many did not make it.  To
make it alone, would make you, the most passionate
and skilled hiker ever, who had simply not gone up Everest.
But they had perhaps gone up, other massive mountains,
and were prepared.  

I'll say this is a second emergent theme, of
business engineers trying to author infrastructure for
their own problem space, instead of selecting an infrastructure.

It also made me realize there is a good classification
to make **business software engineer**, and **infrastructure expert**.
I'm the former, probably a senior business software engineer,
with a good working knowledge of infrastructure engineering,
but armed fully with the knowledge of the difficulty,
and that even after thirty years of this stuff, deep threading
knowledge; I'm simply not qualified.  I could try, and
would make too many mistakes to make it worthwhile, in the end.
But I'm aware of this, and so, I do a good job evaluating
the right tool for the job - and I know when to avoid
inventing a tool which I am not qualified to invent, 
and the enormous difficulty.  A brilliant 30+
year engineer recently asked me "why is inventing this infrastructure difficult",
truly not knowing, the tremendous depth necessary to 
do these things correctly.  I explained it quickly, and it sunk in.

Core infrastructure does look like just another mountain to climb, 
to a mountain climber.  This is not like any mountain you've
climbed though, building an infrastructure. Moreover, you
are likely not qualified if you are not tremendously afraid
of the complexity and cost, and would much prefer to use
something else that someone already authored.

### No incentive to say "no"

Due to game theory, managers are not incentivized to
make these the statement - sorry team, you are not 
qualified to go up Everest, as passionate as you are.

It is not easy to tell your enthusiastic team,
"Everest takes more hikes than we have done."  You want
to believe, you have Everest style hikers, as a manager.
You very likely do not have them, not even one or two of them.
They are incredibly rare, most of them in ultra senior positions,
or retired.  But you need several of them for this kind of team 
hike, including one who has been up there before,
is fit, trained and willing, to go up again, with the current
specific team you have, as the Sherpa.  Any good Sherpa
would first check the climbing skills of the team,
and then ask, why are we going, do we need to go?

### Back to the investment bank

It gets worse for our investment bank team.  Long
before they started their 32 bit, Java RMI cache,
EhCache had long since existed, was mature and in wide use.
No one considered this would be a better alternative.

After being late joiners, a very amazing team mate of mine
decided to benchmark EhCache for the exact problem we were
facing, and a couple of other open source caches, and
documented the results on an internal wiki.  It was
not pretty for the Java RMI cache, because, it used RMI!

EhCache was  free and easy to use, and was many
multiples faster than the Java RMI cache the team had 
built with Spring and Java.  

No documented "buy vs. build" analysis existed,
and this was my first interaction with **"Wobbly wheel 2.0"**,
which we named this system  as a software antipattern.

The team at the bank had re-invented the wheel, 
but it was not round, it wobbled.  Kaveh G. if you ever read this, who could 
ever forget the Wobbly Wheel 2.0 pattern discussion.
I am still on the lookout for Wobbly Wheels!

### Realization sinks in

The project had been named "STP" for straight-through-processing,
because we had a powerpoint which said that somehow this
Java RMI cache was an amazing new technology, which was
being re-used effectively.  The CTO of the investment bank,
trusting his heros, presented it to the entire bank in 
his annual presentation, as an example of the incredible
tech.  

I think the CTO actually believed it when he presented it,
or he unfortunately trusted he was being given good information.
I never said a word about him personally, nor ever met him,
but he was let go by the bank later that year.  I think
the trading desk realized the massive cost of this framework,
realized it was a large and overpriced, slow system,
and blamed the CTO.  I wasn't aware of anyone else being
let go, and I'm not sure if this was the reason, but I have 
a feeling the poor CTO of the bank bet a lot on the success
of this effort and framework.  I'm sure he was a very
sharp person, and trusted, that because the developers
worked in Volatility Derivatives technology for an investment
bank, that they knew what they were doing.  The team were very 
good at Volatility Derivatives (equities), at this bank,
and the developers were very knowledgeable about this 
area of investing, and how to build tech for it.

But this led to the belief by one fairly junior engineer
on the team, that they should build this framework,
and install it globally.  

Which then went badly, to the tune of $100M.  At the $1M,
$10M dollar mark, I bet things seemed fine, and no major
problems had been uncovered.  Not until it went live globally,
not only in one region of the globe, did the core
problems with the tech emerge.  But then since millions
had been invested, they followed it with more millions of 
investment.

The problem with not putting a stop to these things before they
get momentum, is it comes back to bite you, 
years later when it is viewed as a failure which you 
green-lighted.  

It is never fun to be the voice of reason
when the ideas are floated. However, if you let them float,
then you need a plan to change jobs before the two-year
mark when that effort is live and viewed as a failure.

### Cure for the big ambition, junior developer

I haven't found a cure for the big ambition, 
underqualified developer problem, who wants to hike
Everest.

It is one of the very tough organizational problems in software
these big thinking junior developers.

Amazon have the ridiculous idea to encourage this kind of
approach, even naming it "Think Big."  Someone at Amazon
is hoping one of the developers is going to invent the
next AWS.  This is a high cost lotto ticket, as they
encourage everyone to invent things and then managers
to champion half-baked technology they have invented,
claiming that it is the solution to all problems (and
worthy of their promotion.)   Amazon can't tell
the difference between the good ones and the terrible ones,
and are constantly green-lighting ridiculous ideas,
because they are told to embrace "Think Big."

There eventually go into the graveyard of 
failed frameworks and attempts,
all over Amazon, long lists of them.  It is a nice
idea in theory, this "Think Big", but if you lack
the skills, better for these engineers to not build
their Think Big, but to have another team take the think
big and do the right thing with it.

### Unlimited funding, no real goals

At the investment bank, another common problem was 
"too much time, too much money."  In this case,
the volatility business made a billiion dollars, and
had only six developers, and no work left to be done,
but an excessive pool of money to spend on software.
So they were given a blank check.  These blank checks
are a receipe for disaster.  And the next year,
the business lost a ton of money, and then this project
was in trouble, and then eventually canceled.

This "too much money" problem, causes amazing problems
in software.  Developers who don't have enough work,
start to try to invent new solutions to old problems,
but don't have enough qualifications to be doing this.

Once invented, a marketing campaign ensues by
the developer and their manager, as to how great
their framework or technology is.  They have a new
thing, and they think it will get them promoted, and
are of course very proud of it.  This is the most 
dangerous stage, because to tell them their baby
is ugly, is viewed as highly negative, and uncool, 
rocking the boat in the software shop, asking important
questions like:

1. "why would we build this when it exits?"
2. "are we qualified to build this?"
3. "where is the competitive analysis?"
4. "where is the comparative build / buy analysis, documented?"

Even with all of these documents, it is not easy to tell
someone who works for you "sorry, no, we are not doing
your idea."  I dislike telling people, everyone dislikes it.

What I know is, having seen the other side - better to 
say it directly, than to let it grow into a failed project
which you were around, and did not stop.  While
saying "no" may make you a little unpopular, at least
you will not be fired in two years, for having it be 
greenlit.

### Due process

It is a hard problem to stop.  

My solution now is to say this.

Every new infrastructure framework or project should require 
a thorough cost-benefit and competitive analysis,
and a ton of documentation as to why it is necessary,
in a standard format, very long form.  Also, exactly
how long the project will take, and the exact benefits
and why these makes the juice worth the squeeze.

And I mean excruciating detail.  This is now the level
of detail I spend on analysis of any software investment,
because software is under-analyzed, everywhere.  

I do this out of habit, produce these documents, to make sure
the projects succeed.  Anyone who wants to float
an idea should be made to produce them, with bar raising,
and a presentation to a very senior team where they
defend the idea.

This amount of process should be enough to discourage
ambitious junior developers from getting in over
their head, and proposing a new software infrastructure.
(or a new in-house software framework.)

### The end of the Java RMI global cache

At this investment bank, the 32 bit, Spring XML,
Java RMI cache, which cost $100M to engineer and use,
is currently the worst software failure I have ever
witnessed or been a part of.  

The team were moved
to other projects, and the system retired as soon
as the incredible funding dried up, because it was 
just not worth it, and free alternatives existed.

Java RMI was never fast enough for a cache, or usable enough,
to compete with anything.

**Everone believes someone else will speak up about
a bad idea - but no one will.**

Speaking up about a bad idea is like CPR; you can't
expect someone to go get help, you need to ask one
specific person to go get help, or as they teach you in CPR,
no one goes - everyone thinks someone else has gone to get help.

The same applies to stopping bad software infrastructure
decisions and frameworks.  Unless
you know someone has stopped the effort, 
it is up to you to stop it, 
or you are as guilty as the core team who are inventing it,
for not speaking up.


**Stay tuned for the second $100M failure: Distributed Journal Database.**
