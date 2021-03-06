## User Story
---
##### Added on 22-05-2018
---

#### Definition
In consultation with the customer or product owner, the team divides up the work to be done into functional increments called "user stories."

Each user story is expected to yield, once implemented, a contribution to the value of the overall product, irrespective of the order of implementation; these and other assumptions as to the nature of user stories are captured by the INVEST formula.

To make these assumptions tangible, user stories are reified into a physical form: an index card or sticky note, on which a brief descriptive sentence is written to serve as a reminder of its value. This emphasizes the "atomic" nature of user stories and encourages direct physical manipulation: for instance, decisions about scheduling are made by physically moving around these "story cards".

#### Common Pitfalls
* a classic mistake consists, in teams where the Agile approach is confined to the development team or adopted after a non-Agile requirements phase, to start from a requirements document in narrative format and derive user stories directly based on its structure: for instance one story for each section of the document
* as the 3 C's model emphasizes, a user story is not a document; the term encompasses all of the knowledge required to transform a version V of the product into version V' which is more valuable with respect to a particular goal
* the level of detail corresponding to a user story is not constant, it evolves over time as a function of the "planning horizon"; for instance a user story which is scheduled for the next iteration should be better understood than one which will not be implemented until the next release
* a user story is not a Use Case; although it is often useful to compare and contrast the two notions, they are not equivalent and do not admit a one-to-one mapping
* a user story does not in general correspond to a technical or user interface component: even though it may sometimes be a useful shortcut to talk about e.g. "the search dialog story", screens, dialog boxes and buttons are not user stories

#### Expected Benefits
For most Agile teams user stories are the main vehicle of incremental software delivery, and offer the following benefits:

* mitigating the risks of delayed feedback, all the more so
* if the increments are small
* if the software is released to production frequently
* for the customer or product owner, the option to change their mind on the details or the schedule priority of any user story not yet implemented
* for developers, being given clear and precise acceptance criteria, and ongoing feedback as they complete work
* promoting a clear separation of responsibilities between defining the "what" (province of the customer or product owner) and the "how" (province of the technical team), leveraging the skills and creativity of each

#### Potential Costs
Incremental development in general, and the "nano-incremental" strategy embodied in user stories in particular, has significant impacts on projects' testing strategies, and in particular all but mandates significant test automation.

This follows from the so-called "quadratic growth" problem: after implementation of feature F1 it is normal to test that feature. After implementation of feature F2, the risk of regression dictates re-testing F1 as well as testing F2. A test sequence assuming incremental development therefore goes: F1,F1+F2,F1+F2+F3, etc. - this grows as the square of the number of features, and can therefore quickly become unmanageable as projects grow in size. Test automation (in particular unit and acceptance testing) mitigates this effect, although it does come at a cost.

#### Origins
User Stories originate with Extreme Programming, their first written description in 1998 only claims that customers define project scope "with user stories, which are like use cases". Rather than offered as a distinct practice, they are described as one of the "game pieces" used in the "planning game".

However, most of the thrust of further writing centers around all the ways user stories are "unlike" use cases, in trying to answer in a more practical manner "how requirements are handled" in Extreme Programming (and more generally Agile) projects. This drives the emergence, over the years, of a more sophisticated account of user stories.

* cf. the Role-feature-benefit template, 2001
* cf. the 3 C's model, 2001
* cf. the INVEST checklist, 2003
* cf. the Given-When-Then template, 2006

#### Signs Of Use
* the team uses visual planning tools (release plan, story map, task board) and index cards or stickies on these displays reflect product features
* the labels on cards that stand for user stories contain few or no references to technical elements ("database", "screen" or "dialog") but generally refer to end users' goals

#### Skill Levels
An Agile team will benefit if the skills that underpin effective use of user stories are widely distributed among the team; however, it is likely that people with background in "requirements analysis" or with a history in "analyst" roles will have a leg up in acquiring these skills.

**As an individual contributor:**   
**Beginner**
* able to start from another formalism for requirements (narrative document, use cases) and transpose that to user stores
* knows at least one standard format for expressing user stories
* able to illustrate a user story with an example (including user's goal, existing context, user's actions and expected outcomes)

**Intermediate**
* able to divide up the functional goals of a development effort into user stories (possibly epics), ensuring no gaps are left
* knows several formats for expressing user stories and can choose the most appropriate one
* able to express the acceptance criteria for a user story in terms that will be directly usable as an automated acceptance test
* knows or can identify the relevant user roles and populations and refers to them appropriately in user stories
* can assess a user story using the INVEST checklist or an equivalent, and rephrase or split the user story as necessary

**Advanced**
* able to interpret so-called "non functional requirements" in terms of user stories
* able to link user stories to higher level descriptions of project goals (e.g. project charter) and to provide grounds for the relevance and business value of each user story

**Collectively, as a team:**   
**Beginner**
* the team organizes project activities around user stories
* the team is able to obtain, "just in time", the information necessary to implement user stories, for instance by having access to the product owner or domain expert

**Intermediate**
* the team formalizes all activity as work on user stories, each team member can answer at any moment the question "what user story are you working on at the moment"

**Advanced**
* at any moment, any member of the team can answer the question "what is the most important user story in the backlog, and why"