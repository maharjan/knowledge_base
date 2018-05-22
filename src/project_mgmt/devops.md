## What is DevOps?
---
[This article originally appreared on Grahamela](http://www.grahamlea.com/2013/02/what-is-devops-bullet-points-quotes-tweets)
###### Added on 22-05-2018
---

Interest in DevOps is booming. I feel like I heard it mentioned as the motivation for some decision at least once a week last year, and it climbed its way into the headlines of most of the newsletters I receive from LinkedIn, Twitter, InfoQ, etc.

I thought I understood it. It just means Dev and Ops collaborating closer, right? But as it gained more and more attention, I realised it must be a movement, not just a simple idea, so I set out to discover what DevOps was really about.

This blog is my summary of what I’ve learned from reading about DevOps over the last few months. There are heaps of resources and lots of people making great observations, so what I’ve done is try to distil lots of salient points into bullet-point format to make it easy for people to pick up as much as possible in a short read.

If you’re interested in reading my sources for yourself, they’re all listed at the end.

#### Why does DevOps exist? What are the problems that it’s trying to solve?

* Some of the problems in existing IT organisations are:
  * Conflict between teams and inefficiency
  * Buggy releases
  * Failed deploys
  * Teams with orthogonal priorities
  * Teams inadvertently creating more manual work for each other
  * Complexity of application deploys
  * Inconsistency of management controls and interfaces
  * Dev having a “not my problem” attitude to Production
  * Manual administration doesn’t scale

* Some of the causes:
  * Dev wants change, Ops wants stability
  * Separated by management structure, geography
  * Different tool sets
  * Pressure to deliver features above improving processes

* An Agile Dev team releasing on a different cycle to the Ops deploy cycle can result in a Waterfall organisation, in particular extending the feedback loop.

* What we really want is “business agility”, not just software agility, and “IT alignment” – the whole of IT focusing on key business priorities and strategies.

* One of the big problems cited is the idea of Dev “throwing code over the wall” to Ops, i.e. minimal coordination between the teams.

* Some write about “the long, bitter inter-tribal warfare that has existed between Development and IT Operations, as well as between IT and ‘the business'”. (That’s not my experience, but I’ve always worked in small companies where everyone has the “let’s get stuff done” attitude.)

* There’s a long-standing assumption (frequently proven) that IT projects will run late and won’t deliver what was expected/needed.

* Many IT groups have a fear of change, a perception that deployments are risky, and issues from misaligned development and production environments.

* Software projects are actually IT projects, and IT projects have a lot in common with manufacturing processes, especially with regard to coordination between stages to ensure consistent delivery without creating a backlog.

#### What does DevOps mean? How does it propose to solve these things?
* It doesn’t mean combining Dev and Ops into one team, or just getting Devs to do Ops work, or automating a bunch of production config.

* It’s about unified process, making the whole Biz -> Dev -> Ops process one machine that pushes value and feedback in both directions. Really key here is having Ops give feedback to Dev and having Dev do something about it.

* Ops is potentially a stakeholder in every Dev story – invite them to add requirements. Ops is also a Customer that can produce Dev stories.

* Dev is potentially a stakeholder in every Ops story. Dev is a Customer that can produce Ops stories.

* Both teams are responsible for understanding, considering and accommodating the other’s needs.

* DevOps encourages early and frequent engagement between teams.

* Operational requirements are as important as functional ones.

* DevOps is primarily around communication, but some argue that automation trumps communication.

* It’s a culture change, which may require a change to the way success is measured so that success is when both teams deliver, i.e. Dev can’t be achieving if Ops is not, and vice versa.

* It’s about unified tooling: making sure Dev, QA and Ops use the same tools as much as possible, the same versions of underlying platforms and software, the same model of what to change and how. Every tool choice needs to consider the impact on the end to end process.

* Automation is key: machines are much better than humans at executing long lists of complex tasks quickly, in the right order and without error.

* Is it just Ops doing Agile? It’s similar: Agile software fixes business to Dev misalignment, DevOps fixes Dev to Ops misalignment.

* However some see it as being a bit grander: Agile aims to solve efficiency problems in one IT function: development. DevOps aims to solve efficiency problems between and across all IT functions.

* DevOps is NOT a role and that idea is dangerous because it allows cross-functional issues to just become someone else’s problem. You wouldn’t “hire an Agile”.  You can’t “hire a DevOps”.

* DevOps is not chiefly about better tools. It’s about solving the IT alignment problem, of which tools play a part in the solution. Be clear on what your problems are and which ones you are fixing first before jumping to tool discussions.

* DevOps is not just about Dev and Ops, but that is a place where many organisations have a huge disconnect so that’s where discussions of mending IT back into one function have begun.

* DevOps can turn an organisation’s IT capability into a competitive advantage.

* Just as software teams have software design patterns to aid communication and prevent wheel re-invention, teams that manage infrastructure and deployment need infrastructure and deployment patterns.

* Organisations that go a long way down the DevOps path often end up with Ops becoming providers and maintainers of platforms that Dev use as they please, without requiring intervention from Ops. So they’re doing internal Platform as a Service (PaaS), which probably includes but also extends a “Private Cloud”.

* There is a related term ‘NoOps’ which is supposed to embody this idea of a private PaaS and the idea that developers never interact with the Operations team. Others have said this is just an extreme implementation of DevOps and calling it ‘NoOps’ is misleading as there is obviously still an Ops team taking care of the PaaS.

* As more systems move towards being large collections of interacting hosts, managing the system manually becomes impossible and automatic management must become part of the application itself, a feature set that may be developed by Ops, Dev, or most likely a combination.

* The idea of “Infrastructure as code” is emerging as a key pattern where infrastructure operations are executed by automated, repeatable tools using shared and version-controlled configuration, rather than being performed by hand and ad hoc.

* The aim of infrastructure automation is not to remove the need for Ops people, but to allow them to spend less of their time fiddling with tools and more time delivering value to the business.

* The two main (competing) tools in the Infrastructure as Code space seem to be Chef and Puppet, both tools for automating the configuration of large numbers of hosts.

* There is also a big focus in DevOps circles on comprehensive system monitoring. While Nagios seems to be in use widely, those in the know demonstrate that the monitoring tools space is not mature and there is by no means a clear winner.

* There are lots of things Devs can learn from Ops and Ops can learn from Dev. Each team needs to learn to not be afraid of new ideas.

* Dev need to take more responsibility for their applications running in Production. Some suggest that Dev and Ops share pager duties.

* Like Agile development, DevOps is about continuous improvement: don’t try to implement a huge plan for monitoring and automating everything, but make frequent incremental advances towards the next most important thing.

* As ‘polyglot programmers’ is a trend encouraging Devs to arm themselves with many skills, ‘sysadmin coders’ is a trend for Ops to do the same: understand a few tools well, but also have many varied tools on the belt ready to be whipped out for the right situation.

* I think you’re more likely to find a plethora of multidisciplinary, “can do” people in startups, where people HAVE to take on multiple, varied responsibilities or the company doesn’t run. Larger organisations, on the other hand, tend to encourage people to specialise, become experts, and often not “interfere” with things where they’re not an expert. (That’s why consultants are brought in, right?)

* Through communication and shared goals, the whole IT organisation can be pulling in the one direction.

* Getting started with DevOps chiefly involves a change of attitude: a willingness to collaborate, focus on what the business needs and help both teams to move in the same direction together.

#### Best Quotes from Articles I’ve Read
These are some quotes from the literature that I thought were such gold that I knew I couldn’t improve them so I just copied verbatim:

* “Currently, DevOps is more like a philosophical movement, not yet a precise collection of practices, descriptive or prescriptive”

* DevOps is “a framework of ideas and principles designed to foster cooperation, learning and coordination between development and operational groups”

* DevOps is “the concept that developers and operations need to work together as part of a team towards common goals rather than being two warring factions who hate each other”

* “A model where teams work together to produce consistent, high-quality software that delivers business value using a set of processes that are integrated at every step together”

* “Success at scale depends on the ability to consistently build and deploy reliable software to an unreliable hardware and platform that scales horizontally”

* “The end goal of infrastructure as code is to perform as many infrastructure tasks as possible programmatically. Key word is “automation.”

* “Server configuration, packages installed, relationships with other servers,… should be modelled with code to be automated and have a predictable outcome, removing manual steps prone to errors.”

* “What this means is that managing, designing, deploying and testing infrastructure should be done in an analogous fashion to how we do these same things with software. The code that  builds, deploys and tests our infrastructure should be committed to source-control in the same way as the code that builds, deploys and tests our software is.”

* “Continuous deployment is a prerequisite for the high deploy rates characterized by DevOps, and is therefore a needed skillset for the modern DevOps practitioner.”

#### Quotes from the Tyro Team
I asked a couple of people at work how they’d define DevOps in one or two sentences:

* “An attempt to break the introversion of both groups.”

* “DevOps recognises another system in business that needs to be adjusted to new ways of doing business. It improves IT service delivery agility by harvesting synergies, reduce friction points, re-thinking and re-imagining processes and encouraging system thinking.”

* “It feels like the line separating Devs and ops is becoming less defined. To be able to meet today’s requirements of continuous delivery, high availability and security, the people involved need an understanding of each other’s jobs and a big picture perspective.”