# Production Support

A common question I see asked is “how do we do production support in agile / scrum?”. This is a harder problem than it might at first seem. There are three stages to solving it properly.

## What actually is production support?
Production support or maintenance means maintaining a system once it has gone to production, i.e. it has been released to customers. This generally takes the form of fixing production incidents. Something goes wrong and you need to do a production change. We can divide these incidents into two camps:
* Something has suddenly gone wrong and a service is no longer working (or no longer working to an acceptable level)
* A bug has been discovered and is affecting customers.

### Two types of production issues
The first type is generally caused by some kind of temporary infrastructure problem: a server falls over, or runs out of memory, a message queue gets blocked, or a network switch gets overloaded. They are usually non-deterministic (sometimes I try, and login and I get an error and sometimes I don’t), come quickly and go away quickly (restart the server, recycle the application pool). They don’t involve bugs in the code, so they don’t require a code fix.

The second type is more serious: there’s a bug in the code and it requires a code fix. These are usually deterministic (it will happen to the same person every time they try and reproduce it) and they don’t go away quickly (you need to prepare, test and deploy a code fix). How easy the second type is to fix will depend on the Continuous Delivery maturity of your application.
### Who does these fixes?
The first type is usually fixed by an Operations team who maintains the infrastructure on which your application runs. Although maybe not, depending to what degree your organization has embraced DevOps. The second type is either fixed by a software team that built the relevant application, or a dedicated “maintenance” team. That is, a team that doesn’t build new features but just supports the applications that other teams are building.
But how are you supposed to get and stay on top of these issues? I see it as a three-stage process.

## Production support in agile phase 1: Track and prioritise
The first thing you need to do to tackle the problem is to clearly identify the problem. You need to log incoming production issues in some sort of bug-tracking system, with appropriate details, priority, etc. You need to make them visible (to the team and to stakeholders). Your product owner also needs to prioritise these alongside the work the team is currently doing. If your team is purely a maintenance team they should be doing no other work, this may not be the best option for a long term project.
If you are clearly tracking and prioritising your incidents, you’re on the right track. Make sure that the product owner is regularly reviewing this list and prioritising them not just against each other (“this new defect is much more serious than that old one”, but against other in-flight work e.g. user stories for the sprint (“fixing this defect is more important than getting that user story done”). Production defects and incidents should generally go into a sprint as part of sprint planning, but sometimes you should jump on them as soon as they arise (if they are very nasty). Make sure you do not estimate defects or earn points for them.


## Production support phase 2: Own the stack
The next stage is to “own the stack”, which means try and get your team to be responsible for much as much of the stack as you can. To ensure you can resolve issues quickly and the team can build up complete knowledge of their own system, you want to get a fully cross-functional agile team.
If you need to hand defects over to a DBA team or an Integration Services team or a Security Services team, your cycle time is going to fall and you’re going to suffer from a lack of control and ownership. Build up the skills, knowledge and ownership in the team until you control the entire slice of the application. Eventually, you want your team’s remit to include not just design / test / build but run also.
This is part of (but not all of) the journey to DevOps: the people who build the application, also run the application. Doing this will involve overcoming technical and organisational hurdles (mainly organisational) but is necessary. This will further reduce handovers, documents and ambiguity. The reason why a dedicated maintenance team is not a good idea in the long term is that they do not have that long-term ownership and responsibility. In Amazon development teams, their mantra is “you build it, you run it”. Developers won’t put out shoddy code if they know they’ll be the ones getting the phone calls at 2am to fix things.


## Production support phase 3: Optimise the stack
Once your team has ownership of the stack, you need to ruthlessly optimise it, by putting in extensive amounts of automated tests, both functional and non-functional. The key principles are:
* The existing feature set is covered by a full set of automated regression tests
* No new features get in without full automated test coverage
* Every time a defect is fixed, at least one test is put in place for it.

If you do this properly, you will achieve two important benefits:
* You will be able to push changes out to production very quickly with low cost and risk
* Your defect / production incident counts will approach zero.

To clarify: your incidents from code bugs (the second type) should approach zero. You might still have the first type of incidents, resulting from weak or faulty infrastructure. 
