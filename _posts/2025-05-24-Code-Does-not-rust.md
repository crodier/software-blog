## To refactor or not refactor, that is the question --

*Whether ’tis nobler in the mind to suffer
The bugs and code smells of outrageous legacy..*

**Value of refactoring:**
$$V = (F * A) - (R * Z * O) - (F * B)$$

1. R = Time to refactor / rewrite
2. F = Number of features / times you alter this code
3. A = Additional cost to work on the existing code
4. B = Perceived cost to work on refactored / rewritten code
5. V = Value of refactoring
6. Z = Risk of mis-estimating coefficient
7. O = Opportunity cost factor

**V** must be positive to even consider proceeding with a refactoring.

Otherwise, you are investing in an emotionally driven, irrational quest to improve
code "As if code rusted."

By not shipping features during this time, you also
risk the satisfaction of making progress, burnout, 
and other serious business problems.

If you can't estimate these numbers, don't begin doing anything; 
you are probably setting money on fire on an emotional quest
by a developer "I don't like someone else's code, I prefer my own."

(which is how all developers feel about all code, everywhere.)

As engineers, a challenging senior understanding exists,
captured by the [**"Respect what came before you"**](https://www.amazon.jobs/content/en/teams/principal-engineering/tenets)
, Amazon Principal Engineering tenet.

We need to first realize this emotion exists to disrespect what came before ,
the strength of this emotion, and - this is not a rational response.

## Is it time to rewrite or refactor?

I've spent many months and years both refactoring 
and rewriting existing business software (of 28 years total, at time of writing.)

On this topic, my favorite blog (from the year 2000) with the best software advice I've read is "Joel on Software":

**Things you should never do, Part 1.**

https://www.joelonsoftware.com/2000/04/06/things-you-should-never-do-part-i/

The key quote:

> "As if source code rusted."

*Notably, Joel went on to invent Stack Overflow.*

### The "Amazon Pricipal Tenets."

We should review the Amazon Principal Engineer Tenets.

Notably, **"Respect What Came Before You"**

https://www.amazon.jobs/content/en/teams/principal-engineering/tenets

The principals are a good guiding light for any Principal Engineering group.

### Developers always dislike other people's code

Heard everywhere in software "I don't like this code I didn't write.  My code, I like it, and is better than this."

Is your code really better, or is it merely, code you wrote?

As a developer, the urge to rewrite is strong.  Code is never perfect, and can always be improved.

Also, developers dislike code they did not author, always, as a rule.  

Reading code you did not write is usually extremely painful.  
Also, software is naturally a frustrating 
and painstaking human activity.  It feels good to write
greenfield, or rewrite code.  It usually does not feel good to read
and improve, someone else's code.

### Why does it not feel good to read other people's code?

In code, someone else had an idea of how to solve a problem, and took the idea, 
then produced one of many alternatives to solve the problem.  

You lack this thought process and selection of a path to solve the process; only the result.
These many critical micro-decisions, are not what you would have decided, were you there at the time.
This is enormously frustrating.  

Having worked in 18 different shops, and built systems 
where people arrived and criticized my code / designs, there is a key takeaway.

**All developers dislike someone else's code, and all wish to create a more perfect, vision of the code
in their own minds eye.**

Are they right, and when are they right?  The answer is, usually
this is a knee-jerk, emotional reaction, and irrational.
I've done it myself, many times, and then had it *done to me*, 
many times, as the owner / author / inventor
of many large systems.

#### The real question to ask:  

Is there anything truly wrong with the existing system / code?  
Or is it ["just, like your opinion man?"](https://www.youtube.com/watch?v=j95kNwZw8YY)

What are you saying, when you say "this code can be improved."

**Yes, all code can be improved.  e.g. Linux Kernel**

The question is, are the critiques real, do they matter?

Should we invest, prescious time and energy on improving this code, 
against all else?

Because of the strong emotional and irrational, urges to improve code and systems, 
we must be **extraordinarily judicious** in what existing systems and code we choose
to refactor, or completely rewrite.

## Code does not Rust

Code can always be improved.  You have limited time and energy.
You must be judicous about these investments.

Particularly, it is easier to mandate someone else, 
to rewrite code; as you yourself, are not tasked with
producing, a new complicated machine, which does,
the exact same thing, as the existing *working*, complicated
machine someone invented, coded, tested; which already solves the problem.

If **the refactoring formula** above does not hold, you are chasing
emotional "not my code", by developers.  And
when new developers arrive, guess what - again you will hear - "not my code".

I heard it said:

> Beware developers bearing rewrites.

As a manager or senior person; beware!

Those developers are merely saying, what all developers say, about each others code, code they didn't author.

## Case Studies (2)

### Case Study 1:  Java parser, rewrite as XSLT - Lehman Brothers "POINT"

One brilliant developer spent a year, rewriting a Java
system which parsed an XML, and mapped it into an object,
which was then inserted into a database.  This was the
"bond analytics" package of data for a "priced bond" at Lehman
Brothers, in the "POINT" system for bond analytics.  

This was part of
a large distributed, bond pricing and risk reporting system.  

When a bond is priced, the
analytics about the bond (sensitivities to interest rates changes)
are calculated by a "calculator" service, particular to the bond
("Government Bond Calculator vs. Corporate Bond Calculator Service.)

These "analytics" are returned, and need to be 1) parsed
and 2) saved in a database.  

This parsing + saving, was happening in Java, and 
it was low latency, and working quite well; no major problems.

We needed to add one more "type" of Bond to this system,
as a new feature.

Rather than add the feature, 
a decision was taken by the "architect" on the system, 
that we should start over.  The technology which must be used was XSLT,
XML StyleSheet Transformation Language
[XSLT](https://developer.mozilla.org/en-US/docs/Web/XML/XSLT.)

You've never heard of XSLT.  But it was a very hot fad at the time,
in 2004, when XML was also very hot.  Both, now long gone of course,
and clearly fads, even at the time.  Yet, the architect tasked
my team with a rewrite.  What was the reason?  It was cleaner
to use XSLT, a technology "designed to parse XML".  It was
not easy to understand or read XSLT, and no one knew what it was,
but the architect, he was very smart, very senior, and convinced
it was critical to rewrite.  And so, we began to rewrite.

It was expected it would take 2 months.  It took an MIT 
genius programmer, one of the best junior developers ever,
6 months to build and test the rewrite.  And after this,
the developer quit Lehman.  I spent some time with him on 
the project, along with my other projects.  It was terrible.

The system we produced was higher latency, and did not compile (was a runtime parser.)

But the architect / manager 
was convinced it was a good use of time.

The architect was and is, truly, a brilliant and kind person.  

But it was a bad investment, and we agreed later, a mistake.  

It was a good learning for me,
and for him, and the team.  

I started developing this idea about when to rewrite,
and also found the Joel Article, during this effort.

### Case Study 2:  Excel to Java for "hedge fund risk":  Rewrite

After this experience with "rewrite or not", 
I've only been successful (in the large) but once, and I did so
with great hesitation even then, along with this estimation.

We had an Excel workbook used to calculate complex 
financial risk for a major hedge fund.

The hedge fund had hundreds of thousands of holdings.
The sheet had been designed when the fund had a thousand holdings.

Excel, was starting to break down; the Excel at that time.
Even today's Excel, would struggle, becuase we could not 
effecitvely "multi-thread", with the code being in Visual Basic,
inside this Excel Workbook.  I owned the sheet, and needed
 to run it, and send the results at the end of the day to the entire 
hedge fund leadership, including the CEO.

We had major problems most days.  This Excel workbook, when it was making REST calls to 
fetch data, would often freeze / crash, even on a beefy PC.

First, we added tactical workarounds in the VBA code where we would break up certain
calculations.  We added five or ten new buttons in the sheet,
 press them one at a time, to divide the work the sheet was
doing (and then wait, then press the next "go" button on the sheet.)
Even then, failures were common, and we would re-run it, sometimes repeatedly.
We could not go home, until this report was sent to management, 
critical for the investment business.

Every day, due to the problems, one team member would spend about a hour or more dealing with it
as an operational risk function for a major hedge fund,
managing tens of billions in investor money.

### What to do?  I think we better rewrite (gulp.)

We needed the resulting report to be in Excel, 
to allow for changing numbers and "fiddling" with the results.

The workbook had 10 "sheets", each with charts and graphs
of a table of aggregate numbers; where the underlying numbers
were available as raw data elsewhere in the sheet.

The management team knew how to drive it, and formulas connected
the results to the data, in the sheet itself.

### Cost estimate to rewrite

I had used (at Lehman), the [Apache POI](https://poi.apache.org/)
library, where Java can generate and format an Excel workbook.

I reviewed the VBA code in Excel, and there were 10 sheets.
I estimated that I could convert one of them (this was pre-AI),
per day, to Java using Apache POI, and have the exact same result,
tested, taking 10 days total.  

By my math, it would take **80 hours**
invested in the rewrite.

### ROI

As we spent an hour a day 
wrangling with the problem, in about one quarter (90 days), 
we would have "repaid" ourselves this investment.  

We knew we had to 
run the sheet every single business day.

We had also had severe 1+ hour breakdowns in the sheet, some days.

The math seemed to work.  There was opportunity cost;
what other projects would not be worked on.  

We reviewed these other efforts,
and decided to try to invest some effort here, carefully,
to see if we could quickly "get in and get out."

### Time to rewrite - measuring as we go

I bid the two weeks on the rewrite, and my mentor and manager
said "I don't know if you can pull it off, but let's see."

After the first day, I had done the easiest one, and it looked ok.

My manager checked back in on me.  He was happy to see, I 
was able to convert one of them a day (tested), producing
identical formatting, formulas, and values in the result, but
it was generated from a web-page using multi-threaded Java,
and ran quickly.

### Week one down, very tired = 50 hours of refactoring

After a week, I remember being exhausted, with a week to go.

But I took the weekend off, rested, and returned, ready for 
week two.  I pulled it off by the end of the second week.  

We used the new end of day system
the following Monday.

### Results

It didn't feel too amazing; as only we were really aware of the
improvement - there was no business facing improvement.

My boss was happy, and we were happy though as a team
to have this support burden out of the way.  

Then it was back to the 
large list of projects for the hedge fund, which needed
tending to, and were now, two weeks behind!

### Limited success

This was the only time I've been successful.  I've tried
a great many times, probably ten or more times, and never
been able to justify the investment.  Some of those times
*I was in charge*, and able to dedicate my time wherever 
I felt it was best used.  Until you have done this, 
chased and measured your hopes and dreams for code,
and many other things, do you realize this:  

Don't rewrite,
and don't refactor, until you know *exactly what you are getting into*
and *exactly what value you hope to achieve*.

aka:  the refactoring formula, at the top of this document

## Critiques of my own architecture and best work

The problem of developers who want to rewrite, or think
they can do better, has never been more profound for me than 
when I designed, coded, led and launched Exodus Point
Fixed Income software, as a Principal Engineer.

In six months, we built some of the best systems
on Wall St.  This is not subjective.  Portfolio Managers
were hired, from Citadel, into Exodus Point.  Those PMs
 asked, "where did you buy these great systems, these are better
than what we had at Citadel."  To which, our management
told the PMs "we built them here."  the PMs said, 
this is not possible, you just started the hedge fund
six months ago.  The managers said, we have some guys who 
built this, they are friends and were hired to work together.

This was and is a true story, Exodus Point has best in class
systems for many of the things we built.  

I was 25 years into building hedge fund tech, hands on, 
when I started designing and building Exodus Point, and had seen the best 
designs at many of the top hedge funds, producing a synthesis
of the best of these designs.

The results were fairly incredible and done with a small budget
and rapid time to market 
(3-6 months to the initial launch, ~ five developers.)

### The team grows

After the launch, we doubled the size of the team from the
initial core team of about six of us, to about twenty people.

Despite the amazing functionality, spears were thrown at me,
and people continue to question the design decisions, to this day
over beers.  Even with the incredible outcome, and my 
best effort and outcome I can recall,
all my heart and 25 years into this build.

### "This is not perfect"

What did I hear from new joiners:  "This is not perfect."

Another thing you will hear "This could be better."

Are these statements wrong?  No, not wrong!  

It was not perfect, and it could be improved.  

Should we throw out what we did?
No, we should not start over.  The results are working,
and there is no need to start over.  But, these 
critiques were thrown at me.  Incredibly, but some of
the most senior and elite hires we made, had the most 
severe critiques. 

For example, one ultra-senior, ultra-brillaint engineer said:

"Why did you use [Druid](https://imply.io/blog/apache-druid-vs-time-series-databases/), 
a time series database do to this; I can instead, write a time series database, which is faster, and in C++."

This was both said, and was attempted.  

To achieve a minor performance improvement, 
and because it was then in custom C++ (which is better than vendor, Java?)

### Respect What Came Before You

This is why Amazon Principle Engineering, developed 
a virtue called ["Respect What Came Before You."](https://www.amazon.jobs/content/en/teams/principal-engineering/tenets)

I learned this later, when I joined Amazon.

This is an amazing list of Tenents, I had not seen put together before,
but it captures many advanced thoughts about software and software groups.

Respect what came before, is a powerful, profound conclusion.

Software is never perfect but "they pulled it off."

With whatever constraints they had on budge and otherwise,
they built a working solution.

Do you need to rewrite it, or refactor it?
Software is never perfect, so review **the refactoring function** at the 
top of this blog, before you begin.

Otherwise, you are thrusting yourself and your team 
up against, well-known truths about software engineering,
which Amazon Principal engineers have nicely codified.

### Appendix

One more amazing blog from Joel:

https://www.joelonsoftware.com/2009/09/23/the-duct-tape-programmer/
