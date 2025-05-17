---
title: "The Blueprint - software delivery at scale."
date: 2025-05-05
layout: post
published: true
---

# The Blueprint - a new standard for software process delivery [DRAFT]

Planning and designing software is the most critical phase 
of a major software delivery - problem analysis and design.

Why are you solving a problem, why this particular problem, and in this particular way?
How much will it cost, and what is the benefit?

Without well-documented answers to these questions, we must pause before
beginning any medium-to-large software system endeavor.  
Seemingly minor details, for example,  consistency / eventual consistency, recovery from failure, scaling,
and latency, can become key dimensions for your software effort.

It is straightforward to make these decisions informally in a small team setting; 
however, it is then challenging for a team of
more than ten people to review the analysis.  

A key virtue at Amazon
is a step where engineers "poke holes", 
and "actively seek disconfirmation of beliefs", 
at this design and project decision phase.  
This leads to 1) careful project prioritization,
and 2) careful assembly of any non-trivial software effort.  

**Aside**: _Disconfirming one's own beliefs_ is the most difficult
activity for a corporate software professional; asking publicly
"please tell me I am wrong, and why", opening yourself
up to criticism.  Yet, it is the most important activity
for any software architect - this level of confidence
and vulnerability to receive honest and direct feedback.
The key is noting feedback is not about you, but is about
one design you are presenting.  And if you are receiving
feedback it means you have captured the audiences attention,
if even with a "straw man" design, to raise awareness of
the problem.

### Document centricity

Amazon use _documents_
to make software design and delivery "scalable", 
allowing people outside the group reviewing the problem.

Strong documents allow management, along with engineering leaders from across a large group
to make meaningful comments on plans and designs, and ask important questions.

Here we outline refinements to this document driven project 1) selection and 2) design process

This "Blueprint" process can be re-used as an approach to any software system.

### The "Deliberate Software Process"

1. **PRFAQ** - a press release about the project and the benefits to the customers
2. **High Level UML use cases** - a list of use cases which are created or impacted by the software change
3. **T-shirt size cost estimation (high and low estimate)** - estimation of the time and size of the effort - informally
   4. After this phase, a decision can be made if more time investments are warranted
5. **Prototype** - potentially produce a rapid POC of the idea, to further vet it out
4. **Fully Dressed UML use cases** - "fully dressed" UML use cases, using the Craig Larman, Fully Dressed format
5. **High Level System Design** - system design, with alternatives, pros and cons for each
   6. *Following a standard format for High Level Design (draft:  to be documented)*
      7. Primary Section
         8. Development environment creation
         8. Exact APIs
         9. Exact messages incoming and outgoing
         10. Exact systems processing the messages
         11. Deployment
      7. Includes **Mandatory Treatment** of:
         8. Failure and Recovery from failure (crash of any system, outages)
         9. HA/DR
         10. Scalability
         11. Latency estimate p50, p99, p100
         12. Latency at 10x traffic
         13. Cost to operate (cloud and people), and at 10x traffic
         14. Monitoring + Logs + Troubleshooting
         15. Impact on Overall Firm Velocity
         16. "Debuggability" and "Testability"
7. **Detailed System Design** - detailed API contracts, ERD diagrams, messages, and software systems
8. **Deep dives on every system** - reviewed by teams individually before beginning
9. **Detailed project plan** - project plan and more detailed estimates 
9. **Management review"**
   10. Now we pause, and management review. 
   11. Should we start this effort?
   12. What is the **"rank"** against all efforts?
       13. Rank is used for resource assigment — across every team.
10. **Staffing, Project plan, timeline**
    11. Only after the greenlight, staffing considerations take place, and resources are assigned, along with a much more detailed project plan
10. **-- Only now start building --**
11. **Weekly scrum of scrums** - report tracking to expected goal, and any blockers, assistance needed
    12. This references the project ranking.
    13. Status is updated prior to the 30-minute, scrum of scrum every week to senior management, with all managers and PEs in the division
    14. Note: Smartsheet has emerged as a highly effective tool for agile project tracking, and rolling up status across a workbook of sheets
11. **Pre-Launch:  Launch Plan Review + Operational Readiness review**
    11. The system is reviewed to verify it is ready for prod
    12. Includes reviews of operational dashboards
10. **Launch + Celebrate** + Learnings captured; what went well, what can be done better

## Project documentation homepage

One homepage exists, in a catalog of projects being worked on (and completed projects.)

Each of the documents listed above, is linked to the document listed.

This catalog serves as documentation for new joiners, allowing them to 
quickly understand what was built and why.

We can call this the **"Project Catalog"**, but it is also the **"Library"**
globally for the business; a key artifact to own, for ongoing operation
and growth of the systems.

A **librarian** is assigned to do a once monthly review, and clean-up.

This way the documents continue to match, the actual system in production.

## Conclusion

Software is extremely expensive to build, test, and own.

Using a rapid documentation process, and wide review, the most important 
software projects can be built quickly, and after having been reviewed by 
a broad management and engineering team, in a very deep and meaningful way.

This is in contrast to a more informal "green lighting" of a software effort,
after a meeting or discussion.

With this level of documentation, the engineers are pushed to think through
the problem, and to play the chess game before it begins.

Engineers often believe this is a waste of keystrokes, as I did for much of my career.

I give credit to Amazon for their deliberate approach to software (and teaching me),
where the process of writing pushes the engineering team to perform very detailed
analysis, and to effectively perform multiple iterations on a plan, 
but not as software iterations - before the software has even begun.

This leads to a much better first version of a system - the equivalent
of a second iteration of the code itself, at a small fraction of the cost,
merely some docs and a few hours of review - which can be done in days.

It leads to the two most important things in software:
1. Starting and green lighting only the right efforts
2. Building them rigorously, front loading the analysis

**The documents are easy to write, once it is systematized.**

One brief training video can bring anyone up to speed on why and how to 
follow the process.

The value is considerable to any medium or large software group, at Amazon scale.

