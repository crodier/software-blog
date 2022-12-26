# Outage Process
This chapter is about a consistent set of steps any team will use during an outage.
The steps and the timing are important, becuase they let teams in both the softare and business organzation
observe exactly what is happening and who is doing what, and see if they can get more help for the situation.
Once the exact details are agreed upon, a published version and a meeting shoud be held company wide
where the documentation is reviewed and an example incident run through, either recent or a fictitious event during the training.
This can be recorded on the Zoom call, and connected to the wiki.  Once this is recorded, any new joiner to the 
organziation shoudl review the outage procedure.


## Why process it is important?
Explain why it is important to have a process during a software outage.  You need a standard which is well understood and consistent across your group.
Having a well known standard allows other engineers to observe the outage and potentially help out without interrupting to ask for details.

Without an outage process developers who may have helpful information may not be able to locate the group working on the outage.
Executives can't observe what the team is doing.  Sales and any other customer facing teams will be unable to 
let customers know the severity of the outage or any details on potential recovery timeline.
In an emergency, communication is key.  Here we will tell you the right amount of commincation and the set of channels, how and when
to communicate.  Most importantly, having this process in places leaves the engineering team free to do focus on the outage itself.
It is preferable for a non-engineer to handle the communication but if it is an engineering focused group, the most senior engineer 
generally will take on communications or ask for another team member to handle it while they focus on the outage.
Another benefit is by having the process in place, you don't need to discuss, what is the right amount of communication
regarding the outage, and how often to provide updates, as the standard is already known prior to the outage occuring.
You would not want to show up to a fire and not know how who was communicating the progress of the fire and how and when.

## How other organzanizations handle communications during emergencies

TODO:  How do firefirghers and surgeons communicate during emergencies, and how does the military.

## Communication channels and timing

Slack is used by many organziations.  We will use the messaging chat app named Slack here as a placeholder for whatever enterprise messaging chat system you have.  Slack is used by many organzizations and is excellent for wide scale immediate and targeted communication.  The features 
are also available in most open source chat apps like Mattermost.

The Slack system has an extremely useful feature called a channel.  One channel should exist for each major business group, as their "outage" channel.
Minor areas within these may have their own outage areas for more detailed communication.  The main company outage channel should be reserved for only outages which impact everyone, but it is importante these channels also receive the same summaries of the situation.  When new joiners arrive they should typically join the "#company-outages" channel and one or more "#division-outages" channels.

For our purposes we will say there should be a channel called "#company-outage", and another "#division-outage", where "company" is the name of the 
company you work at and "division" is a name of each of the business units in this division.

At the start of an event, it should be determined if it is potentially a large scale outage.

## Determining Large Scale Outage

A large scale outage has different criteria for every organization.  Divisions may have their own criteria for large scale outages.  It is possible that 
an outage meets the criteria for a division or company wide outage, the communcation should be used on the outage channels.  Not every outage will 
impact the entire company.  For a large dot-com, an outage will typically impact only application offered by the company.  Outages which impact
an entire dot com are much rarer.

## Change Management - All changes via code

A chapter about how in HFT, changes are only made, even to configruations, via code which is checked in, reviewed, and tested.
This way the procedure is documented for doing something, adn then a script performs it, leaving little or no room for 
human error.  Major causes of human error are misunderstanding, and then understanding but not performing the set of steps correctly.
This can be due to typos when entering the commands, or missed mouse clicks.  Some of the largest outages in history 
have been caused by unscripted changes performed incorrectly.  The "Knightmare" incident where Knight Capital, one of
the oldest and most presigious trading firms in New York went out of business in one morning.  A firm which took over 20 years
to create, lost $425,000,000 (million), dollars before noon on a Tuesday (?) in 2009.

### Knight trading change management details
If the change had been applied correctly and in a scripted manner, then all eight servers would have been patched and running the new Power Peg system correctly.  The incident would have never occured.  7 of 8 servers were patched with new code, where when a flag was turned on "PowerPeg=True",
the new "PowerPeg" portion of the system was turned on.  A developer had chosen to take a flag which was already in the code and re-puprpose it.
The developer had left Knight Trading at the time of the incident, but the management team failed to force the developer to use a different flag.
The developer shoud have invented a new flag, which could have been "PowerPegNew" or similarly named.  This would have been the correct course of action.
The problem was that the old code prior to the system being released, used "PowerPeg" to enable a system for testing code.  This test code was an 
automated stock buying algorithm, which would try to accumulate any stock which was traded.  For example, once the flag ws turned on, if you
bought one share of Google for a price for 89.23, the test code would start buying any Google stock orders it could find; if anyone sold Google 
shares, at any price, this test code would start buying as much Google as fast as possible, without a cap.  

The first mistake was allowing any test code into the production systems.  This test code should have been in a test package, which was put
into another distributible file, eitehr executible or otherwise.  This test executable should have never been copied to the production system, and 
it should have been forbidden.  This shows the most basic lack of software process or controls were in place at Knight Capital.
Often times software teams "break the rules", because they are in a rush and think they can tolerate the risk of the system.
The difficulty happens when those developers are no longer with the firm.  It must be the expected for any software written today that 
the system will evolve and the deveopers who invent the software will not be operating the system years later.
This is a situation where it seems like the investment is not neccessary in these controls.  But the controls are trivial to introduce, 
taking hours, and eliminate entire major classes of software and business risk from the organzation.  


## Outage timeline
