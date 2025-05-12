Two people in the world know, maybe three now,  I helped invent Strata, the OpenGamma library for pricing financial derivatives.

It's a superb library, kudos to the team who designed and authored it.  I did not author it, but I sketched it in a whiteboard, for the now CTO, when he was getting in started in industry.

My biggest failure as an Engineer led to their success, maybe as an entire organization.  the now CTO was a junior developer, a year out of university, brilliant.  This was in hmmm, 2012 or 2013.  long ago.  We were w client who had signed up to use Open Gamma 1.0.  Strata is 2.0 effectively.

OpenGamma 1.0 was an extremely complex software system, in Java.  It was Java, and my boss asked me to evaluate it to replace FinCad, an expensive commercial library we used, million dollar license fees.  considerable spend for a hedge fund.  FinCad is an awesome library.  It just is not cheap, and you can't alter the pricing models, which is a big deal on a pricing group, at a hedge fund (financial instrument derivative pricing.)

Strata is good use of UML design for finance, modeling as software, and some slick coding and date math work.  It is not a small problem, but modeled well, is straightforward enough.  there are calculators which take Instruments as inputs, and produce analytics results.  It is a good read if you have time.  I was able to suggest exactly how this should work, after having done FinCad for a long time.

Prior to Strata, my hedge fund was an early adopter of OpenGamma.  Some brilliant quantitative mathematicians had built it "quants.". But in 1.0, they were focused on building "a platform for portfolio pricing", and not a library.  Essentially, in building a framework for pricing a portfolio.

They had designed ths system where you store all the configuration of how you want to price a derivative in database tables.  the system would read your portfolio, look up how to price things via the tables, and voila.  seems fine right?

This one extra layer of indirection between the desire to do something in code, price a bond, an a lookup or bond pricing conventions in a database, followed by calls to prove the bond, proved to be fatal for the design.  The problem is debugability.  

Debugability is a quality of a system.  If it takes a long time to debug a problem, something is very wrong.  It took days to debug simple problems, due to layers upon layers of indirection, and errors not bubbling up.  Pricing can become many levels deep, to price one thing, first you price something else, an observable or first derivative financial instrument.  this is used to price a second, and so forth.  it becomes highly nestef at times.

For this system, **debugability** was paramount.  Why did I get number X and not Y out?  This is not easy to track in. calculation system.  Goldman Sachs solves this problem and much success followed.  They cracked debugability as a first order of business.

Due to this one or two extra layers of indirection, you could not determine what was happening in the call chain.  We took 30 to 40 hours to trace small problems, with maximum focus, and the original authors on Zoom calls.  They themselves, could not debug version 1.0.

We thought we could replace FinCad in a year.  The project was more than five years long, and lokey used the rewrite I proposed.  Open Gamma flew the now CTO out and I spoke to him, gave him these thoughts and how I would do it.  and then he ran with it.  that is how Strata happened.  go me!  It was rather matter of fact frustration at the time.  I'm proud of the thinking now which went into this analysis of how it needed to be done.  A lot of real suffering and stress trying to make it happen with OpenGamma 1.0 led to me thinking if a better way.

The problem is, I signed up for OpenGamma 1.0.  my boss asked me if it looked ok, and I said after a brief look "it is a Java system, how hard can it be?".  Turns out, very hard, impossible.  This lack of analysis, cost is the five years into the effort.  My boss and mentor trusted me.  Asked me to review it.  In my defense it was n informal ask, because some other people had said it was good.  they were more fact checking with me.  

But I had an opportunity, to try to actually learn it, and try something, and give real feedback.  I did not, and probably cost this fund ten million in developer costs.  I built them HFT infrastructure which was outstanding and they were not suffering, but let me say, I think to si the worst thing I have done, in my career analysis wise.

Today, I wikdm of course never make such a massive decision without due process and review.  Anyway in 2012, I thought anything in Java would be ok, and I could deal with it.  I know now to be afraid and look before I leap.

As a team lead, with other much smarter people, I was trusted to make this recommendation, and it looked ok to me.  The analysis was far too shallow for such a complex and important problem and project.  It is my own example of "half ass analysis", which I could never forget. 

 A year of absolute suffering ensued.  three months in, I said wait, we need to abort, this is going badly.  But we had told too many people and signed consulting services agreements.  We were stuck.

I think this thing happens, and on software it should be expected.  if you don't take any risks you don't learn anything, and you can miss major opportunities.  When it does happen, don't chase the good money with the bad, do abort.  A bad software project never gets better, it gets worse and worse.

It feels good to skip the analysis, and start coding !  Don't do this.

Overinvest in decisions which will be expensive.
first you need to do a cost estimate if the decision impact.

You can't invest heavily on every decision. but slow down on the big ones.








