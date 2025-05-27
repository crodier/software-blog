---
title: "Ticketmaster Design - Case Study"
date: 2025-03-18
layout: post
published: false
---

# Design Ticketmaster:  Interview Design Case Study

[HelloInterview.com](https://www.hellointerview.com/) 
is an amazing resource.  Credit to the team
who built it, I certainly learned a ton reviewing this site.

Among things well done, are presenting the interview and
what areas to focus on; for example, *strong vs. eventual consistency.*

Coming from finance, strong consistency is almost always used,
and this seemed like a trivial concern.  I've since learned
that sending events and eventual consistency is a popular 
paradigm in dot com, often used when Elasticsearch / OpenSearch
are introduced, for scalability reasons and search; however,
sacrificing strong consistency.

Second, read vs. write heavy designs is another interesting
area of system designs you may not have focused on, but 
makes sense for many global scale system designs.

### Keys to this problem:  Functional Requirements

When your adrenaline is pumping, the most important thing
is to understand the **functional requirements first.**

My biggest problem during interviews:  not jumping into 
the design, and discussing functional requriments.  

Many interviewers though, view these as obvious for many questions - 
it depends on the type of question.

In the ticketmaster problem - the need
for a waiting room is the primary functional requirement.  

If you don't realize you need a waiting room, 
you design the wrong system.  It is a bit binary
for my taste, as to, if you stop and realize the waiting
room exists (or remember.)

This failure to understand the problem, happens to be
the biggest problem in software in general, making it 
a fairly amazing interview exercise.  Except that in 
real life, you spend at least a week on a design of this
complexity, ripping it apart and trying to improve it.

I like the problem, but it is very focused on making
this assertion of the Waiting Room, and then building correctly.  

This realization may or may not occur with adrenaline pumping

### Keys to the Ticketmaster Design

**Waiting room** -- the design calls for a Waiting Room.  

If people don't buy the ticket count in time alotted,
then users are selected out of the waiting room (in strict order),
and then allowed to buy a ticket.  This means there is a 
queue needed per event; a persistent queue in the event
the entire system crashes and everyone re-connects.
It also calls for a **high speed counter** -- a mildly interesting problem

We should put a certain number of users in the waiting room,
and then, once we have more than 3x the people in the waiting
room as tickets; close the waiting room, and give a specific
page of "Waiting Room Full, pls. try again later."  This becomes
a "Sold Out.", once the event is fully sold out.

**Bursts:**  When a Taylor Swift concert opens for business, we may
need to handle a burst of 3 to 10 million calls happening.
How can we block the traffic from coming to the app servers,
once the waiting room is 3x full?  Here, we want to change
the webpage returned when people arrive at the site, not
allowing entry - prevent anyone from trying to get in.

We should have a general throttle, and a user specific 
throttle; preventing users from trying to get in repeatedly.
If users must sign in to get in line, then you have a user
token, and can prevent denial of service by a user.

**Counting at low latency** We need a counter to assign
an order to everyone who arrives in the waiting room.
This could be a Kafka partition offset; but then, we have
only one partition involved — but can we do better? (hint: yes.)

We will have an atomic counter in memory, with a neat trick 
it it crashes, where it can get a new counting offset from 
a database on startup; incrementing the "reboot counter."

**Websockets**  (Websocket / QUIC / SSE) is needed to transition
the user out of the waiting room and to the ticket purchase
experience, or to the "SOLD OUT." page, if the event sold out.

Whenever Websockets are involved, you now must map the userId
to the websocket connection -- typically a Redis cluster,
allowing you to find users and push messages back to them
by finding which server they are connected to via Websocket.

**Searching for specific tickets** is important.  

How do you scale people browsing tickets, which tickets are available?

The "ticket section" needs to be scalable, per event, and it should scale very 
high.  It could be a massive venue, perhaps 1,000,000 users,
but likley not 10x, searching for tickets and looking at
sections in the venue.  It needs to incorporate people buying
tickets which you may have wanted to buy!  e.g. you select
tickets 1-4, Second 201.  Someone else buys them, at the exact 
moment.  You must be rejected and allowed to select four
alternate tickets.

**General Search** for events by location etc, which is a trivial
problem handled by ElasticSearch / OpenSearch, and really
need not be discussed in design.  Searching for *tickets*
during the checkout process is interesting, as other users
are grabbing tickets while you are; and you need to be 
given an opportunity to grab other tickets.

**Timing out** the checkout process is interesting;
you need to give the user a fixed amount of time, for example, 
five minutes to check out.  You want a notification when
these timeouts.  Redis has a TTL, and you could try to use this.
But if you do, then you need to constantly query redis to see
if the TTL has elapsed, to see if tickets are avaiable.

You would prefer to write a custom Java server with 
a PriorityQueue, "peeking" at the top to see if messages
have expired until you find one which is not expired; then 
sleep for maybe one second, try again.  Once you find
expired messages, you can see if checkout is done, and if
not, close the Websocket or similarly.  You need 
to be careful that payment has not been sent to Stripe,
and you are waiting on Stripe to respond.  If the time
elapses and then Stripe responds the tickets are purchased,
you must ignore the TTL expiration, in an important data-race
in this system.

### Notes

1. Waiting Room
   2. A persistent queue is necessary; and it must scale - Likely Kafka
   2. You need a callback to let people in, from the waiting room
   3. e.g. if all tickets are vended
   4. Everyone starts in the waiting room; how does this scale?
   5. You have a fleet of Websocket Servers, mapping the queue position to the Server in redis;
      6. This allows you to "callback" when the person comes out of the queue
      7. The Websocket servers; can auto-scale horizontally and be in any region
      8. You can then service as many tickets as you are capable with the rest of the architecture
   9. As long as you are in the waiting room; there must be one ticket available in the stadium (event.)
   10. If the event is sold-out when your turn comes, then you are sent to a Sold Out page.
   11. You need to keep your browser open, to see if you get in off the queue
   12. Interesting is the counter - a real time counter service can be used
       13. The real time counter can be in-memory; if the server is restarted, the counter jumps
       14. The counter server gets its "restart count" from a zookeeper cluster, or Apache Flink may do this
       15. How else would you count?
2. Sharding on eventId
   3. Sharding is necessary for events; and you would shard on eventId
   3. Sharding should be the most important thing, and would be based on eventId
   4. Otherwise, databases could be overloaded when a tour was released, with multiple dates at one venue
      5. e.g. Taylor Swift
   6. Amazon DSQL would work; like an auto-sharded Postgres SQL
   7. You would likely want consistent hashing, if you sharded yourself
6. Kafka for creating Events
   7. The events would be dropped on Kafka; then inserted into your OLAP and OLTP stores

### Hello Interview Review

Well, the interview process is a necessary evil, and,
Hello Interview does a nice job.

Feedback is warranted though, as it is after all,
a video about how to do great on this problem:

1.  The high level design is not useful
   2. The high level design says nothing.  
      3. There's CRUD and a database.
      3. This is discussed as the high level design; but we have said nothing.
      4. CRUD on top of a database is the design for half the systems in the world; e.g. JHipster designs this for you as a template, and we have said nothing at this point in the video.
      5. Presenting this naive a design as a first high level design would be bad in a real interview.

1. Locking / Reserving is totally wrong
   2. Two step; query then filter is proposed, where the users holding locks on ticktes waiting to checkout, use Redis.
   4. The two step query + filter, would never be used.  
      5. You would never query a database, then check to see if seats were "locked"
   3. Latency is not discussed as a non-functional, up front.  If you don't design for latency, your system will be slow, and it may require a rewrite, should you wish it to be quicker.  This could be called shift-left performance design.
      4. You need to think if 1 second is ok, or if you want 300ms response from servers; 
      5. these are often, very different designs.

2. Seat-updating via Long Polling - no
   3. You would not use seat updating via long-polling.
   3. You would be multi-plexing thousdands of updates, to sections of stadiums no one is looking at.

1.  "Virtual Waiting Queue"
   2. At the end of the interview, they discuss a Redis queue as a virtual ordered waiting room
   3. They discuss using a Redis Queue; 100 at a time.  
      4. Why are you batching off the queue, why a batch size of 100?
      5. There is no discussion, just "take batches"
      
5. Focus on "ElasticSearch", for search
   6. A deep dive is devoted to, using ElasticSearch for free text search
   7. This is not a deep dive
   8. CDC (Change Data Capture) is discussed; but this is not terribly important
      9. Debezim would be mentioned here by a senior candidate
      10. Kafka would be another option, with ingestion by both systems off of Kafka
