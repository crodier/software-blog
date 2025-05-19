Two people in the world know, maybe three now,  
I helped invent Strata, the OpenGamma library for pricing financial derivatives.

It's a superb library, kudos to the team who designed and authored it.  
I did not author it, but I sketched it in a whiteboard, for the now CTO, 
when he was getting in started in industry.

My biggest failure as an Engineer led to their success, 
maybe as an entire organization.  The now CTO was a junior developer, 
a year out of university, brilliant.  This was in hmmm, 2012 or 2013.  
We were w client who had signed up to use Open Gamma 1.0.  Strata is OpenGamma, v 2.0.

OpenGamma 1.0 was an extremely complex software system, in Java.  
It was Java, and my boss asked me to evaluate it to replace FinCad, 
an expensive commercial library we used, million dollar license fees.  
Millions is a considerable spend for a hedge fund.  
FinCad is an awesome library.  
But it is expensive and propriatry; you can not alter the pricing models.
This lack of custom coding is a important for a financial engineering, pricing group, 
at a hedge fund.

The Strata library is strong use of UML design for finance, modeling as software, 
and some slick coding along with date math work.  
It is not a small problem, but modeled well, is straightforward enough.  
The library consists of calculators which take Instruments as inputs, 
and produce analytics results.  
I was able to suggest exactly how this should work, after having done FinCad for many years.

Prior to Strata, my hedge fund was an early adopter of OpenGamma.  
Some brilliant quantitative mathematicians had built it "quants." 
But in 1.0, they were focused on building "a platform for portfolio pricing", 
and not a library.  Essentially, in building a framework for pricing a portfolio.

The team designed ths system where you store  the configuration for pricing a 
derivative in database tables.  
The system would read your portfolio, read this configuration from the tables, 
et voila, price your bond.  Seems fine right?

This one extra layer of indirection between the desire 
to do something in code (price a bond) and bond pricing conventions in a database, 
followed by calls to prove the bond, proved to be fatal for the design.  

The problem is **debugability**.  

Debugability is a quality of a system.  

If it takes a long time to debug a problem, with a debugger, something is very wrong.  

It took days to debug simple problems, due to layers 
upon layers of indirection, and errors not bubbling up.  

Pricing can become many levels deep, to price one thing.
First you price an observable or first derivative financial instrument.  
This result is used to price a second financial derivative, and so forth.  
This chain becomes highly nested for many fixed incomd derivatives.

For this system, **debugability** was paramount.  
Why did I get number X and not Y, is a question you must answer? 
This is not easy to track in any calculation system.  
Goldman Sachs solves this problem and much success followed.  
Goldman sovled for debugability as a first order of business; knowing a-priori it was necessary.

Due to this one or two extra layers of indirection, 
In Open Gamma, one could not determine what was happening in the call chain itself; the trail would end, and start somewhere else.  

We took 30 to 40 hours to trace small problems, with the original authors on Zoom calls.
The authors themselves, could not debug version 1.0.

We thought we could replace FinCad in a year.  
But the project took more than five years, eventually using the rewrite I proposed.  
Open Gamma flew the now CTO out and I spoke to him, 
gave him these thoughts and how I would do it.  He took the ideas and ran with it.  
This is how Strata happened.  

It was born of frustration at the time.  
I'm proud of the thinking now which went into this analysis.  
Real suffering and stress trying to make it happen with OpenGamma 1.0 
led to me thinking if a better way.

The problem is, I signed us up for OpenGamma 1.0.  

My manager asked me if it looked ok, and I said
after a brief look "it is a Java system, how hard can it be?".  
It turned out to be not only very hard, it was impossible.  This lack of analysis, cost is the five years into the effort.  My boss and mentor trusted me.  Asked me to review it.  In my defense it was n informal ask, because some other people had said it was good.  they were more fact checking with me.  

I myself had an opportunity, to try to actually learn it, 
try something, and give real feedback.  
I did not, and probably cost this fund ten million in developer costs. 

Later, I built them HFT infrastructure 
and the fund did very well, but let me say, I think this decision was the worst mistake
I have made, in my career - analysis wise.

Today, I would never make such a massive decision without 
due process and review.  But in 2012, 
I thought anything in Java would be ok, and I could deal with it.  
I now know to be afraid and look before I leap.

As a team lead, with other much smarter people, 
I was trusted to make this recommendation, and it looked ok to me.  
The analysis was far too shallow for such a complex
and important problem and project.  
It is my own example of "half ass analysis", which I could never forget. 

 A year of absolute suffering ensued.  Three months into the project, 
 I said wait, we need to abort, this is going badly.  
 But we had told too many people 
 and signed consulting services agreements.  We were stuck.

I think this thing happens, and on software it should be expected.  
If you don't take any risks, 
you don't learn anything, 
and you can miss major opportunities.  
When it does happen, don't chase the good money with the bad, do abort.  
A bad software project never gets better, it only goes from bad to worse.

It feels good to skip the analysis, and start coding !  Don't do this.

Over-invest in decisions which will be expensive.

First you need to do a cost estimate if the decision impact.

You can't invest heavily on every decision. but slow down on the big ones.






