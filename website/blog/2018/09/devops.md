# DevOps

## What is Develops at Global Kinetic

I recently attended a Meetup hosted by guys from GitLab. They flew down the entire company to Cape Town, roughly 300 people, as part of their annual retreat. They are the biggest remote only company in the world. They attribute their solid DevOps culture to this success.

There are 2 components at the core of their DevOps culture, these being **transparent communication** and **documentation** and below is an expansion on these 2 terms, how they are applied, and how they apply to GK.

### Transparent Comunication

At Gitlab they encourage open communication through a companywide slack channel. At GK we had been using [Slack](http://slack.globalkinetic.com) and now migrating to [Teams](http://teams.globalkinetic.com/). Below are some of the rules they employ to make enforce transparent communication:

* **Discussions about tech should be global,** except when its project specific i.e. no direct messages between people when asking for assistance or sharing knowledge on a specific tech topic. This is facilitated by the [GK Teams Tech channel](http://tinyurl.com/y7avrt9q).

* **Visibility around projects and work,** creating visibility around what people are doing in their various projects in the company is essential to creating a sense of belonging and common purpose.  This allows others in the company to have access to that knowledge pool for new tech they may be interested in or have experience in but are not aware is being used at GK. The Project Health Audit Template shared with each GK team as part of the Devops reviews requires teams to provide a High-level Design which shows transport protocols, tech and 3rd party services. when curious about what others are doing, visit The [GK wiki](http://wiki.armorica.gk/) which provides a good source of info for GK projects, workflows and standards.

* **CI/CD Transparency** at Gitlab they allow every Developer to have complete access to all their Jenkins jobs. A Dev can push his/her latest changes directly to their Production environment. This is only possible because they have checks and balances in place to make sure code passes unit test coverage thresholds, linting/code quality standards, code is versioned to their standard, feature toggles and rollback strategies are in place. Total access is not yet the case at GK as we are still trying to create a successful Devops culture, having various projects both external and internal also complicates this process, as opposed to Gitlab where they are working solely on their own product.

### Documentation

Oftentimes, IP around a project and its peculiarities resides in someone’s hippocampus (not a school for aggressive water vegans). This can create maintainability issues and results in longer times for on-boarding of new peeps, problem solving and delivery.

At Gitlab, they have a basic rule on documentation which is,“ **DOCUMENT EVERYTHING!** ”.
At GK, we already have a wiki  with various sections and it is open to anyone to **add**, **update**, **critic** or **modify**.

Some rules to follow to enable Devops Culture in this area include:

* **Document any solution that can be applicable to someone in the company**
* **Document sprint artifacts** like architecture drawings from design sessions.
* **Document productivity hacks** like clipboard managers, password managers, [markdown previewer](http://markdownlivepreview.com/), [git ignore generator](https://www.gitignore.io/) etc. This helps in maintaining common tool-sets and standards.

* **Document Tech Meetings.** Presentations should be made available to download and this helps create history and continuity. Tech meetings are archived in the [Tech Meetings](http://tinyurl.com/ych3udgg) Teams channel.

## Devops best practices and where we stand at GK

Some practices have clearly risen as being critical to the realization of DevOps' core principles:

* Agile software development - **This we do**.

* Continuous integration - **This we mostly do.**

* Continuous delivery pipelines - **We are getting there.**

* Automated and continuous testing - **We are getting there.**

* Proactive monitoring - **We are getting there.**

* Improved communication and collaboration - **We mostly suck.**

In conclusion, DevOps is not a role held by one person, but requires everyone in the company to play their part. Once these DevOps practices are instilled in everyone, we will be on our way to
flawless software deliveries and improved traffic on Cape Town freeways.