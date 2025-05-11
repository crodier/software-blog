# J2EE resurgence - modularity

Once upon a time I was at a bank called Lehman Brothers in technology.  
It was my first job in financial technology, and maybe my best.

The group of talent included:

1. [Eric Poirier](https://www.linkedin.com/in/epoirier/)
2. [Nat Luengnaruemitchai](https://www.linkedin.com/in/nat-luengnaruemitchai-43aa14/)
3. [Robert Durie](https://www.linkedin.com/in/robert-durie-7bb70733/)
4. [Ajay Hariharan](https://www.linkedin.com/in/ajay-hariharan-3399697/)
5. [Marc McQueeny](https://www.linkedin.com/in/markmcqueeney/)
6. [Chris Winquist](https://www.linkedin.com/in/chris-winquist-117b211/)
7. Chris Sturhan
8. [Ken Yanovsky](https://www.linkedin.com/in/ken-yanovsky-cfa/)
9. Tim Eiffert
10. [Jerome Hauser](https://www.linkedin.com/in/jeromehauser/)
11. Phil Rha
12. [Brian Paulsen](https://www.linkedin.com/in/brian-paulsen-6675b847/)
13. Winnie Howe!
13. Yours truly

Lehman was a serious investment bank.  They hired well, and
we had a fierce group of talent.  

I pulled up the rear in this group, hello friends!

## A magical J2EE time

In these early days still of Java on the server, Weblogic from BEA was
a million dollar "container" like Tomcat, but you started it first, and
then copied your application into it.  They only got one thing wrong,
and only partially; we didn't need to start every service in the J2EE, 
every time we started the container.  Also, you had to connect a 
remote debugger to the container, instead of just pressing "Run" 
in IntelliJ, to start up the app.  This is annoying when it is your
everyday life, little things.  But wow, they were minor; and things have
since then, deteroirated into Microservices.

But wow, did J2EE do the hard things beautifully.  The entire world runs on Servlets.
Servlets and web-apps were 
the main "deployment unit", to build a J2EE app, 
which everyone used, for everything.

But there was another;  EAR files.  An EAR file was "enterprise archive resource."
In the EAR, you could put these other interesting things:  EJBs. 
Enterprise Java Beans.  You can think of them as an ultra complicated gRPC
interface, which you had to pre-compile in those days.

In this post, I will explore the incredible thing EJBs got right,
 which, to my knowledge, barely exists if at all in gRPC, and 
is never talked about in Spring, and it is this.

There was a requirement where if you deployed a Stateless Session
Bean in a container, and it was called by something in the container,
it used local memory semantics.  But if you made the same call,
via Java Remoting (which was awful but worked), then it would know this
and handle the serialization and deser for you.  I think I've seen
one poorly supported gRPC effort which does this, but I've never seen it in use.

This idea is the pure gold, where you have multiple "deployment models" for your app.

The most important deployment model, is developing!!  Everyone must be able to deploy everything,
you could call it "one EAR", one app to rule them all, a composed monolith for development.

Then at runtime, if you truly need to deploy something elsewhere, you do this, 
and it behaves the same as your local dev environment.

Everything new is old, old is new.  Microservices, have no notion and
no framework to deploy "together" in container, nor is it a virtue which is
talked about outside dark infrastructure circles.

It could be brought back to life, and would be a powerful paradigm, with gRPC,
and a deployment model which supported it.  **gRPC2EE.**

Not too difficult to build really, given a little investment.

But start with the Modular Monolith, keep it simple.

Heed these words, heed them well.

https://medium.com/@dileeppandiya/the-evolution-of-microservices-past-present-and-future-57303e758eb4

Microservices are overused.  

Services have been a necessity for productivity sake,
when there were clear and obvious organzational and functional
boundaries.  If you try to draw these yourself and 
they are not clean boundaries, you can run into big problems.
Latency is another major services / microservices problem.

We can note:
1. If you need low latency (everyone wants this), you want to run in one service.
2. Your own developers must develop in one service; to run the platform on their machine.

Therefore, you **must** build a modular system, capable of 
running on a single developer machine, above all.

If this system can easily be split into Services, 
then use the Service Locator pattern.

But make sure, the system is **trivial** to start
and run for a single developer, to work on the system.

Once this deteriorates, your entire velocity will
deteriorate with it.  And it becomes impossible to
reconstruct a development environment, once it is undone.

Focus on the ability for one developer to be quick - 
across your entire stack.  This is the key to velocity.

If you can draw boundaries in the app, and services,
and use Service Locator, then this may help.  But it 
many not help, and velocity depends on running the system - 
one developer, working on any part of it, **instantly**.

Clone --> Build --> Run --> do work.

As soon as this is more than 15 minutes, Developers
will be frustrated trying to work on any system.

Make the hygiene excellent, then Modules, using Service Locator.

Then and only then, can service boundaries be drawn,
but they should not be drawn before this.

