#### Disclaimer:   
This article series was written by Mr [Michael Douglass](https://medium.freecodecamp.org/@mikedoug) on codeburst.io   

## 1. A Journey of Understanding
[Original Article](https://codeburst.io/microservices-architecture-e6907b97a42a)

In the quest to always explore new things and expand my mind, I decided I would discover what the Microservices Architecture movement was all about. I started this new journey in a very old-school way, I bought a book. Specifically I picked up [The Tao of Microservices](http://amzn.to/2pmifTF) by Richard Rodger.

Honest Moment: It took a lot to move past the author’s continual pot shots at developers who fail to adhere to his one true way — the first third of the book is replete with cynicism and down right anger and hatred towards all things non-microservices. While I agree with the fundamental message that he is carrying, I simply do not agree with the persistent negativity.

That being said, and the fact that I am only 62% through the book (thank you Kindle for that nice percentage!), the author so far has done a great job of explaining what microservices are, the mindset necessary to approach them, their benefits, and more.

#### So, what is a Microservices Architecture?
Fundamentally, based on my understanding thus far, it is an architecture where things are **small islands of functionality accessible only by well defined messages**. That is the nutshell, at least.

These islands are deployed independently, and they sit atop an underlying messaging infrastructure/platform that abstracts away the complexities of load balancing, service location, etc. This provides you the ability to code simply in your services, and to obtain operational goodies including highly distributed, vertical and horizontal scalablity, etc. These are traits that the underlying infrastructure provides (though you must be aware of them in your service coding).

#### But I have been OO for so long — Object Thinking?
I happened to have read Object Thinking by David West immediately before beginning the Microservices book — I, therefore, have an interesting set of juxtapositions running through my head. Object Thinking is not necessarily Object Oriented Programming as the masses will know it — it is a strict interpretation of how Object Programming should have been done all along.

##### A Functionality - First Focus
In Object Thinking, we are asked to become “Domain Anthropologists” exploring the rich world of our problem domain to understand the functionality we will later implement. It truly is the job of an anthropologist because you have to go in with no preconceptions, and you have to engage actively to understand unstated requirements (the art of active listening!). Requirements go unstated not because stakeholders are malicious; requirements go unstated because they are so fundamental and obvious in the domain.

?>According to West, from this domain-first perspective we then map the functionality into objects for implementation. Functionality typically takes the form of methods.

This is not unlike what I am understanding from Richard about Microservices — here he commands that we make messages the starting point of the design. What messages must you have in order to completely required functionality.

?>According to Richard, from this message-first perspective, we then map the functionality into services for implementation. Functionality typically takes the form of messages.

In both areas of study, we are called to consider the functionality required and then asked to map that into our implementation hierarchy. In Object Thinking that is mapped into objects, and Microservices it is into services.

##### Great Interfaces Make Great Objects and Microservices
In Object Thinking, David tells us that we are to keep objects short and to honor and obey the encapsulation barrier. This is one of the biggest things I adhere to as a software developer no matter where I am writing code — the encapsulation barrier is the most important thing to protect and a good interface with which to interact is an absolute must.

Richard tells us that Microservices gives you the encapsulation barrier for free because you can only communicate between services through a message. Microservices place the highest importance on the definition of the messages. In fact, Richard goes as far as to tell us that the downside of the monolith is that it is too easy to cheat and break barriers, and that you never have enough architects on a project to keep it from happening. I agree.

##### Deployment
In a monolith, you are typically building the kitchen sink and all of your functionality into one artifact. You then deploy that. There is a lot you can do to have zero downtime and good-clean rollback if something goes wrong, but the fact is that you are rolling out a complete update of the entire code base with your monolithic deployment — your code, someone else’s code, all rolled into one big bundle of joy. If any part of it needs to be rolled back, you are typically looking at rolling the whole thing back.

In the Microservices world you deploy each service by itself. You can deploy a new version of a given service and load balance only a portion of the load onto it as you monitor it for any bad behavior. Over time, your confidence will allow you to turn up more instances of the new version and ultimately turn down all of the old versions. While you are busy orchestrating that, someone else is orchestrating deployments of new code they wrote for services for which they hold responsibility.

##### Testing
Testing a monolith can be a pain. Depending on your language and platform, you are going to find yourself needing to figure out how to mock and test your code. It is perfectly doable, but in every project I have ever seen there have always been a lot of trade-offs made. More times than I wish to ever admit, the code coupling is so great that the unit tests I observe are really integration tests across broad swaths of code.

In the Microservices world, messages provide the bulk of input and output from the majority of the code. Code for which this is not true is code that talks directly to a data store or provides a client API. Those edges are relatively trivial to mock when they occur, and messages provides you the simplest mockable interface — you can send any message you want and you can validate any message you want. This is the first time I have seen an architecture for which testing looks simple.

---

## 2. Early Thoughts Before That First Step
[Original Article](https://codeburst.io/microservices-architecture-early-thoughts-before-that-first-step-fecc2ef9d64)

There is much to consider once you decide to start a Microservices Architecture environment from scratch.

##### That Strange Sense of Déjà Vu
In the Real Life Example section of my article, Complexity in Software Architecture: Decompose for Simplicity, I talked about a past project and how we decomposed it for simplicity. Through my recent reading about Microservices, it has become obvious to me that we created this application using some principals of Microservices.

The five listed systems (services) were completely decoupled from one another by what we called the Event Bus. This bus utilized ZeroMQ as the underlying communication system, and I spent a good amount of time writing a Python object layer to provide many of the things you need for a good Microservices layer:

* **Service Registration/Service Location**: Built in to the event bus layer was an internal service used to both register yourself as a named service and locate other named services.
* **Request/Response and Publish/Subscribe**: Both of the common mechanisms available in a Microservices architecture were available and fully utilized by all services.
* **Multiprocess**: While the software itself was written to run on a single machine, we still used ZeroMQ’s ability to communicate over network sockets so that we could expand our application across multiple processes.
* **Transport Independence**: At any time the use of ZeroMQ could have been replaced with any mechanism we desired. The abstraction through the Event Bus system made this highly possible.
* **Independent Development**: While everything was still kept in a single repository, and we were a single team of three people, we were still able to develop on separate parts of the system with great amounts of autonomy. One developer worked a long time on the upload/download logic of the Sync Engine itself while the rest of us developed the other services.
* **Pure Service Testing**: Each service was tested entirely on its own. There was never a need to spin up an instance of another service to test the proper functionality of any single one.

The beauty of the Event Bus system I built was that, to the Python developer on the team, most of the details were hidden by the library. For each event or publish type you wished to send and/or receive, there was a simple definition file in the form of a class which encoded internally the named service responsible and all encoding/decoding needs.

Once the Event Bus was written and the kinks worked out, writing the software using it proved immensely easy. Most of the time you never even thought about it. In the code which worked with the event, you trivially created an instance of the class and asked the Event Bus to send it.

##### Caveat!
In case a former colleague who worked on the same system reads this, yes we did have to pay attention to deadlock issues since ZeroMQ utilized a single thread to read inbound messages. We did not have the library automatically spin processing onto separate threads; we decided to let smart developers decide what they needed. Specifically: “If what you need to do is fast, just do it; if not, marshal your own darn thread.”

The other caveat was that this Event Bus was the FIRST bit of Python code I had ever written. I had just finished Learn Python The Hard Way shortly before starting, and start I did. Have you ever looked back at that “first code” in a new language or environment and thought, “egads”? Yes, this code was of the egads form.

However, it was a solid rock upon which we built one of my proudest pieces of work. Unfortunately the service is now shutdown and we did not get to continue further developing the desktop sync engine, but I am convinced that what we setup in that project was an architecture that would have kept us going for a very long time.

##### Back To The Microservices…
Now that we have walked a few moments in my past, I want to contemplate what that means for embarking upon a new Microservices architecture. What are the key capabilities I want to be able to provide to my body of developers?

1. **A means of locating a specific service**. Pattern matching seems like the most common approach Microservices supporters recommend, so this means having the ability to supply either specific or aggregated patterns for all available services in the environment to any potential client. There are several ways to do this: A) hard-code aggregate patterns to endpoints; B) provide a service registry which supplies live patterns with endpoints; C) provide a messaging middleman who relays messages to the appropriate endpoints.
2. **A means of balancing load for a given service**. Breaking a monolith apart only to force a given service author into a single server, single process environment feels wrong. Services should be able to run in a variety of load balanced manners. Again several methods exist: A) use an actual load balancer in front of your services; B) provide client-side logic to switch between multiple endpoints; C) the messaging middleman can hold that logic.
3. **A means of providing services in various regions**. We want to build solutions which are highly available. This is an extension on the ability to locate a service and implies additional ability to try different locations if a first location fails. You could have this as a part of middleman service broker, but the best implementation is to include the intelligence in the client.
3. **A means of documenting services**. Ideally, services can be self-documenting in terms of: A) what patterns it supports and B) the network protocol used. Done right, I want to be able to ask a service through a message (e.g. {‘role’: ‘user’, ‘__meta’: ‘interrogate’}) to discover what patterns it handles along with human-readable documentation on each pattern’s use. I will be honest, this is a want and not a need.

##### My Largest Struggle
A benefit touted broadly by Microservice enthuisiats is the ability to author services in varied languages and environments. The problem with this is that if you put too much of the above functionality into a common infrastructure library, then you are building a barrier to entry of other languages participating in the environment.

If you google for Microservices architectures, you will find many code bases claiming to provide you with the underpinnings of such an architecture; I am always left confused after reviewing each one on how exactly this helps keep the architecture open to the use of other languages and environments. Certainly you use standard protocols for the underlying infrastructure (e.g. service location), but the bar for someone to use a new language is much higher than one that already has a fundamental library to register or locate a given service — the new language person has to write that code themselves.

##### Forward Momentum
So the question before me is exactly what level of the above items we need and how to build them incrementally so that we can begin taking advantage of a Microservices environment now. In a world where you must start somewhere, my mind is to hard code endpoints and use load balancers as needed. As the environment begins to grow and take on more responsibility, I would envision growing it to being more dynamic and more supportive of more advanced functionality. Implement etcd as a service locator, or something along those lines.

---

## 3. It Takes A Platform — Eureka!
[Original Article](https://codeburst.io/microservices-architecture-it-takes-a-platform-eureka-97f61af90d5c)

As I enter deeper into the world of microservices, I do so on a quest of understanding.

In the last two days, I experienced a breakthrough while reading Microservices: Flexible Software Architecture by Eberhard Wolff. I really like this book — it took me a bit, but I really do. The first two parts of the book goes over the motivations and basics of microservices along with what, why, and why not. These two sections give a good baseline of what we are talking about and it gives a fairly honest view of the downsides, risks, and why you should not use microservices.

The only problems I had with the first two parts of this book was the high level of repetition, and the regularly missing comma. The repetition, I am certain, is there to help people learn — the missing commas only served to remind me of: Eats, Shoots & Leaves. I really should read that book again. It is a favorite of mine.

It was in the third part of the book titled “Implementing Microservices” when I could not put it down. I found myself murmuring, “yes! yes!” People began staring at me, but I carried on. In was in chapter 7 where I found the clarity that I have been seeking. It was the place where I found my Eureka!

##### Microservices Architecture Platform Requirements
It is absolutely necessary for you, as the architect of a microservices system, to set forth fundamental requirements for any services placed into your environment. These are requirements which the teams in your organization must fulfill in order to place their service into production.

A bare minimum must include logging, monitoring, and deployment. But you can, and should, go further and specify other infrastructure requirements:

* Any data stores must be incorporated into data backup and disaster recovery plans.
* Service discovery and load balancing practices to enable the entire ecosystem to be discoverable.
* Security and resilience requirements.
* If provided, a common messaging and eventing system can be made available for ease of development.
* How is configuration handled: Deployed with each service, or provided through a platform configuration system?

The need to have a baseline set of requirements is simple avoidance of chaos. If you left all of these fundamental decisions down to 10 different microservices teams, you would have 8 different ways to locate services, 7 different logging systems, and 3 different monitoring systems with minimal coverage of failures.

Obviously I make these numbers up, but the point is that if you are responsible for the overall organization, then you need to ensure that you have defined the platform for microservice development. You establish the rules in which microservices teams connect to the environment, and you provide tools and pieces of infrastructure to actualize the platform.

##### Starting the Platform
Now that I am starting to look at my job as providing a platform within which teams will work, I am starting to think about what are the things that I want to provide — rules and tools. The rules will be the requirements we decide upon from the above. The tools will be a technical implementation of some portion of the rules in a chosen platform language. These tools will ensure that teams can instantly begin working without having to write code for these requirements.

This is the list of things that I am considering. These are not all things that would appear from day one, and their implementations are likely to grow and expand over time.

**Logging** — This would definitely be a built-in part of the platform infrastructure. It would likely consist of Splunk along with technical documentation on the format of messages and configuration of forwarders. Format specifications should include identification of the microservice, time, and severity. There should also be very specific security logging requirements including formatting. Every microservice call (both inbound and outbound) must be recorded; a shared infrastructure can perform this, but if direct connections are utilized the microservices themselves must participate in the logging.

**Monitoring** — There would be strong rules around monitoring and the platform infrastructure would, by necessity, provide the monitoring system. A core requirement for monitoring will include traceability of service requests through the environment (aka message flow). The message flows through the system are an important monitoring datapoint — especially correlations between related messages where ratios remain consistent for a healthy system.

**CI/CD** — As a standard and a tool, this is a fundamental requirement. You must have automation on builds, testing, and ultimately deployment. Without the tooling in place, there is no way that producing new microservices is easy. A microservices platform is failing if producing new microservices is not easy.

**Service Location** — I have been around the block plenty of times to know that service location may not be a big thing up front, but that can change suddenly. As the number and diversity of services continues to grow a service locator will prove invaluable. If the microservices cloud is going to be highly elastic (new instances coming online/going offline dynamically), a service locator may be necessary from day one.

**Load Balancing** — If we decide to have load balancing be done client-side, then a part of the platform will be providing a technical specification for how to do load balancing, and an official implementation in a chosen platform language will be provided. In the platform implementation, this may be baked together as part of the service locator. (Look at me violating single purpose without writing a single line of code!)

**Configuration Services** — If we decide to use configuration services as a part of our infrastructure, I would expect us to include one as a part of the platform infrastructure. We would ideally not want different teams to setup their own configuration service and end up with 30 different solutions. I have to figure out where I sit with the concept of immutable server before I would agree to provide this as a part of the platform.

**Messaging Infrastructure** — If we determine that message passing (request/response, publish/subscribe, etc.) are important, I could easily see us hosting one as a core platform service. This would enable teams to use messaging easily without having to setup their own infrastructure. This might not preclude a team from setting up and using their own if the need arises. However, if your system is going to utilize a great number of events, having a unified messaging platform would be ideal.

**Messaging Standards** — Each microservice should not reinvent the wheel in terms of how messaging is done. They can have flexibility, but standards around using messages, events, and REST API endpoints would be best in order to provide a framework which teams can use to begin their work.

**Authentication and Authorization** — Every system needs it. Internal users. External users. Etc. There needs to be a system to provide it, and there needs to be agreed upon standards on how to validate authentication and how to enforce authorization. Authorization should be handled at the individual microservice.

**Key Management System** — Microservices will need access to secret keys and passwords. Placing these in the code or in traditional configuration systems is asking for trouble in the long run. Key management systems abound today, and the platform should provide one to the microservices ecosystem.

**Testing** — Standards around unit and integration testing is a must. CI/CD pipelines should be considered incomplete without some level of testing to fail builds dynamically. Teams do not want to push faulty code into production, and the platform should help them.

**Documentation** — The interface for a microservice is tantamount to a contract. If you know and understand the interface to a service, then your ability to consume it is high. When the interface is undocumented, you leave others the difficult task of intuiting and troubleshooting their way to being able to use your service. (I recently had the unfortunate experience of needing to work with Scala Lift code… the Lift documentation is abysmal with just a class structure as documentation.) I have a dream that the services would include an API for retrieving the documentation for the service-in-production; this ensures that the documentation lives with the code and not in an external documentation system.

##### Tools for Microservices in Diverse Languages — The Sidecar
One of my early struggles understanding this requirement is how it goes against the idea that a team can choose any language they desire for their microservice. The sidecar is the answer to this problem — the team will write their service in their chosen language, and they will run a sidecar process in the platform language that provides this base set of functionality to it. How this looks exactly I have not fully formalized, but it makes sense to me.

My only concern is how bulky that sidecar becomes. Two existing frameworks I have discovered through some amount of searching have been JVM based — including Netflix’s own. The idea of running a JVM for services which are not JVM based feels wrong. The JVM has always felt heavy to me, and it feels like having a friend drive an 18-wheeler to the grocery store for me because I want to ride my bike — we do need somewhere to put all of those groceries, right? Nevertheless, the sidecar concept is a completely viable option.

---

## 4. From Idea To Starting Line
[Original Article](https://codeburst.io/microservices-from-idea-to-starting-line-d6e8cd5e9bb4)

Over the last two months, I have invested most of my free time learning the complete ins-and-outs of what the microservices architecture really entails. After much reading, note taking, white boarding, and many hours writing, I feel like I have achieved a level of understanding such that I am ready to take a first step. Allow me to share what I have learned from start to finish.

#### Microservices: High-Level, What Are They?
Microservices is an architecture in which different component pieces of a software design are created and housed as individual, isolated services. Each is deployed separately and they communicate through well defined, network-based interfaces. Microservices are intended to be “small” (loosely defined) and kept to a single bounded context.

There are many benefits to microservices. Because of their isolation and strict requirement to communicate through well defined interfaces, microservices prevent quick and dirty solutions often found in monoliths. These hacks inside of a monolith result in a loss of cohesion and an increase in coupling — two primary causes of complexity. Many will argue that you can maintain this behavior in a monolith. In reality, because it is easy and because there are too few architects working in our code bases, monoliths typically fall due to this very failing.

?>Complexity comes from low cohesion and high coupling. Microservices provides the structure to keep that at bay.

This benefit cannot be understated. Because we keep the complexity monster at bay, development on systems a decade old can continue to move along at the speeds of development when the system was brand new. Time and again, the complexity brought on by loose cohesion and tight coupling have been the cause of slow development on older projects. Cohesion and coupling is traditionally the technical debt grasping onto our feet, slowing us down. Pile enough of it up over the years, and you will be slogging through it.

When the services are written with them in mind, and the infrastructure provides it, other benefits can include: horizontal scalability, testability, reliability, observability, replaceability, and language independence. The downside for microservices is that to achieve these benefits, you must provide an underlying infrastructure which supports them. Without that support, you can easily find yourself with an unreliable and opaque system — or you find yourself reinventing the reliability wheel in every single service. This is a great lead-in to the next section…

#### Microservices: High Level Requirements (The Macro)
An environment which supports microservices fundamentally needs a set of baseline requirements to ensure some level of sanity. If you are going to run microservices, your organization must be willing to bear the overhead of starting and supporting them. The overhead will not be insignificant. It will take time and money to do microservices well.

A successful microservices architecture must have an internal committee or group responsible for defining the macro architecture — this will define what infrastructure will be provided for the development and operation of microservices along with policies which all microservices must adhere. This committee must be the strongest of your development staff, and it may even be one or more people who do not even work for you yet.

?>The macro architecture is one part provided infrastructure and one part policy requirements for all microservices.

Each organization’s macro architecture will be unique. Each area listed below is completely open to negotiation around where to draw the line for your group: You can provide the teams with a fixed service or library of code to provide the required functionality. You can either mandate its use, or make its use optional. You could simply provide acceptance criteria to which a microservice must adhere, but provide no implemented library or service to help fulfill the requirement. Lastly, you could choose to do and require nothing for any given category.

?>Choose wisely what you leave out of your macro architecture. For every choice you allow the individual development teams to make, you must be willing to live with differing decisions, implementations, and operational behaviors.

You are the committee, and it is always best when people in the organization make these decisions — therefore, I cannot provide you with a baked manifesto. As you are starting out, it is also important to keep this macro architecture documentation open to change and receptive to the needs of the teams and the business. Now, let us turn to looking at the different categories for which macro architecture decisions must be made.

##### Continuous Integration/Continuous Delivery
Core to the concept of microservices is the ability to build and execute tests in a very fast manner. Every commit to the microservice should result in a tested build. Once the tests pass and the build system is happy, a push button or an automatic deployment to production is the next important aspect. Cutting time to deploy allows rapid iteration and enables any number of good coding practices.

This is an easy one to fulfill these days. There are any number of build systems which provide access to pipeline builds. Team City. Bamboo. Jenkins with Blue Ocean. Try them out and pick one. For the most part, the feature sets are fairly standard across the leaders of the pack.

An organization should strive for consistency in how services are built and deployed. Therefore, the macro architecture should define the build tool and pipeline processes. The teams should have a voice in the conversation leading to the choice, but they should not be allowed to go rogue on this one.

##### Virtual Machines/Containers
Hand in hand with CI/CD is the ability to spin up a number of instances of a specific version of your service. The macro architecture needs to consider how teams will manage doing this for both development, test, staging, and production environments.

For staging and production you are often faced with the desire to do canary roll-outs with trivial roll-back in the event of a failure. Having common infrastructure, policies, and procedures around how you package and deploy a service will make this easier for development and operations.

Load monitoring and instance control management should also be considered and facilitated by this portion of the macro architecture. How to determine when more instances of a given service are needed, and having a consistent way to put them into production will be critical to long term success.

##### Logging
It is vital to monitor your microservices in production. To do so efficiently, you need to enable quick location of disparate information. This implies that the macro architecture should strongly consider including the following:

1. A logging service for centralized logging. This can be the Elastic Stack, Slack, Graylog, and others of their ilk. You want a logging stack that includes a strong parser/visualizer because you are going to be dealing with a bunch of data. Part of your infrastructure can be one of these services, and a guarantee that each host in the environment will be configured to transfer log files on behalf of each service.
2. Definition of trace IDs to enable the location of all logs across all microservices handling a single external request. The concept here is that, for every external request coming into your microservices, you generate a unique ID, and that ID is passed to any internal microservices calls used to handle that request. Thus through a search for a single trace ID, you can find all microservices calls resulting from a single external access.
3. Base formatting requirements for server, service, instance, timestamp and trace IDs.

##### Monitoring
This is another “must provide” for the macro architecture. Microservices will each need to decide on the best metrics to measure and monitor which will ensure individual success, but the macro architecture will have specific instrumentation it will need from every service in order to provide oversight of the overall health of the system. Some macro level data points include:

* Volume of messages, failures, successes, retries, and drops.
* Latency of requests.
* The ratio of received messages to sent messages.
* Status of circuit breakers.
* And more. Much, much more.

Instrumentation is one area where The Tao of Microservices really shines, and I highly recommend it for a good understanding of the breadth and depth of monitoring in microservices.

##### Service Registration & Location
This is often overlooked when a microservices architecture is small, because a few microservices can always find each other relatively easily. However, as time goes on and the number of microservices grows, the configuration necessary to connect everyone together statically becomes too constraining and eventually error prone. Many solutions can be had including DNS and configuration services (etcd, etc.)

The macro architecture of your microservices environment must define how this is done — even if the first iteration is /etc/services.yaml deployed and synchronized to all hosts. This is not something that the development teams on individual services should set in place — it should be ubiquitous and managed from the macro architecture level. There are many, existing open source projects attempting to solve this problem including some of the service mesh software listed at the end of this article.

##### Communication Mechanisms
Microservices should have some level of control in how they implement their interfaces. Both the network level protocol and the application level protocol should provide some level of flexibility. Using Google Protocol Buffers over raw TCP could be just available as using JSON RPC over HTTPS. That said, the macro architecture should provide some guidance, some restrictions, and maybe even some infrastructure to help facilitate communication.

If a microservices infrastructure is going to work together in a common domain name space under HTTPS URIs, then you will want standardization around the naming and routing. The requests should have a common and consistent method by which ingress user requests as well as service-to-service requests are authenticated, authorized, and routed.

A microservices infrastructure which wants to permit the use of messaging as a communication device should consider providing an operations-managed messaging bus. This enables rapid development and deployment of services without teams needing to first focus on starting and then long-term managing a messaging service. It also fosters decoupling of services which want to communicate through the messaging service; if I have to know which messaging queuing service each service uses, I am growing more coupled.

Providing the infrastructure for your messaging layer also enables you to provide message routing to your services — something which can greatly enhance the flexibility of your macro architecture. The ability to route requests through different versions of a service based on various criteria affords a lot of flexibility and helps to further maintain decoupling.

##### Load Balancing & Resiliency
Microservices are often used in environments where scaling and availability are expected. Traditionally, network devices provide load balancing functionality, but in a microservices environment it is more typical to see this moved into the software layer of the macro architecture’s infrastructure. Code through which services communicate can utilize service location to discover all network locations of a given service, and it can then directly perform load balancing logic right there at the distributed edge.

Resiliency means remaining stable even in the face of errors. Retries, deadlines, default behaviors, caching behaviors, and queuing are a few of the ways microservices provide resiliency. Just like load balancing, some parts of resiliency is a perfect match for the infrastructure to handle at the edge — such a retries and circuit breaking (automatic error responses for services exceeding a failure threshold in the recent past).

However, the individual service should consider what resiliency role it should play internally. For example, an account signup system, where losing a signup equates to losing money, should take ownership of ensuring that every signup goes through — even if it means a delayed creation that results in an email to the account owner once successful. Internal queuing and management of pending signups may be best managed directly by this mission critical service.

##### Persistence: Database, NoSQL, etc.
A microservices architecture completely isolates each microservice from the rest. Ultimately, they understand their own data storage needs best, and should therefore be individually encouraged to control their own destiny as it relates to data persistence. However, you still do not need to allow the wild, wild west to rule the day, and thus the macro architecture should provide guidance (sometimes heavy handed).
Here are some options you can look to include in the macro architecture:

1. One or more data storage services including an SQL based relational database and a NoSQL storage system. These provided data storage services should include built-in backups. A microservice should utilize unique credentials with limited access to a schema restricted to only that microservice’s data. In this scheme, the operations team providing the storage service are responsible for its operation.
2. If you allow the microservices to bring their own persistence, you should have strict policy requirements for backups and disaster recovery. Think about off-site backups, recovery time, fail-over time, etc. In this model the development team is responsible for the operation of the storage service.

You should absolutely, without a doubt, refuse to permit the traditional “open access, one database to rule them all” mentality which permeates world of monolith development. If your disparate services are able to communicate through the database, unexpected coupling will occur. Services must only have access to its own data stores, and cross-service communication must be maintained through their well-defined network interfaces. I recently stumbled upon extremely nasty coupling of the database sort in an older monolith; the complexity was immediately obvious and my sadness grew exponentially.

##### Security
Your services need to know to whom they are talking (authentication) and what data and operations are permitted (authorization) to said identity. There are several potential concepts here:

* Let the IP network protect the services — if you run all of your microservices on a protected network, and you want to transfer trust to your development staff to not abuse access, then this might work for you. Keep in mind that a breach of a single service implies full access to all other services.
* Service-level authentication — shared keys or certificate based authentication allows a called service to validate a calling service. You will need a secure way to distribute and update keys and certificates to keep this secure. Use a Key Management Service.
* User-level authentication — not only are services talking to services, but they are quite often talking on behalf of a user or even directly to a user. There must be a means of authenticating and authorizing the user-level credential to the resource at hand.

Start simple — this is an area that can break an organization out the gate, and it is probably best to start simple. You likely already have a few different services that talk to one another, and you are likely using some IP access-control lists to protect them. Start simple, add to the complexity as a natural evolution of the system.

##### Amendment X — Reserved Powers
?>The powers not delegated to the infrastructure by the macro architecture are reserved to the individual services respectively, or to the developers of such.

Do not underestimate the power of this statement. If the macro architecture does not cover an aspect of the environment, the developers are free to choose and choose they will. The more teams you have, the more solutions you will find yourself maintaining. Therefore, do two things with your macro architecture:

1. Consider very carefully what you leave out. If you follow the “start small” principle, you are likely not going to be providing a lot of ready made infrastructure to cover the details of the macro architecture; this is perfectly acceptable. However, you can still provide guidance and requirements around those areas in order to minimize the chaos.
2. Iterate rapidly. As the first few services come online, meet and discuss the entire macro architecture. What is working? What is not working? What do you need to change now? (How very agile of me!) Do this on a regular basis. You will hear this again in a few moments.

##### Who Should Use Microservices?
?>Everyone should use Microservices.
There, I said it, and I will defend it relentlessly. Yes, I realize that there are plenty of people, likely far smarter and more learned than me, who state, philosophically: “If you are not Netflix and you are not Amazon, then the overhead of using a microservices architecture is going to drown you.”

?>The notion that I have to be Netflix or Amazon to make productive use of a microservices architecture brings, and I hope you quote me on this, one word to mind: Hogwash.

##### It’s All About Size…
The reality here is that the smaller your organization, the smaller your needs for a fully fledged microservices architecture. However, there is no reason to abandon the entire movement and leaving behind the benefits these very smart people have realized, even when you are a small shop with small services.

Your initial microservices macro architecture conversations need to focus on precisely what you need to get started, and then figure out how to get that into place. Build some services, observe their behaviors, learn from what is and is not working for you. Reconvene your microservices macro architecture committee and use your new found experience along with your healthy reading and growing understanding of the industry-wide ecosystem to determine what the next evolution of your macro architecture must be. Rinse and repeat. Iterate.

?>Your microservices macro architecture should continuously evolve right alongside the every day, iterative development you already do.

We live and breath this “agile” world of iterative design and development. There is very little reason that it should not apply to the infrastructure surrounding our services. Even if you never actually realize a fully idealized microservices architecture, but you have these architecture conversations and continually add small iterations of infrastructure and macro architecture — you will have reaped many of the benefits over time. Most importantly, because you focus each iteration of the microservices macro architecture from a position of what you need at-the-time, you will have spent your time on the most valuable components to your organization.

Perhaps you started with a healthy CI/CD pipeline that took over 85% of your existing monolithic development jobs. Dividends! Next you standardize your deployments into docker images and provide tooling around launching, migrating, and rolling back new versions. Dividends! Then add in consistent logging and monitoring, and you start to visualize and report on messaging flows through your systems. Dividends! Now as you are adding new services you realize that the coupling of services talking directly to one another is holding you back, and you add a messaging service to your infrastructure and begin moving some functionality to event-based triggers. Dividends!

I do not believe you need to be Amazon or Netflix to reap the benefits of a microservices architecture. In some cases, you can use the knowledge of how these architectures work inside of a single monolith, and the dividends can be quite rich indeed. From the start or years after the monolith begins to fail under its own weight, you can use the knowledge of how to separate services to shore it up and make it more stable. A monolith which is internally designed with good separation between services makes an easy target for microservices when success demands more from it. (Just realize that it takes architects to maintain the integrity of a monolith, and beyond the start of a system it will be difficult to achieve long-term.)

##### The Macro Architecture Infrastructure
One of the key questions when I began this journey was how I would provide any desired, baseline infrastructure to the developers of services within my organization. My reading lead me to understand three primary methods:

1. Run systems which provide the services along with documentation on the proper use thereof. An example here is to provide a CI/CD system and guidelines on how to configure your service’s pipeline. This is perhaps the simplest of the two because we are all very used to having this type of prepared infrastructure managed by an operations team.
2. Provide code which developers can bake into their systems to perform the desired functionality. An example here would be a shared library that can be used to perform service location and load balancing. This restricts the ability for teams to choose their own language, but the benefit of not creating this infrastructure multiple times can outweigh that cost.
3. If language independence is truly desired on your services, the infrastructure components can be placed in a sidecar implementation which runs as a secondary process alongside each service. The sidecar then represents the service, and provide access to other services, in the infrastructure. Sidecars appear to be more prevalent in the industry than I had first thought possible.

##### Off The Open-Source-Shelf Infrastructure
There is a plethora of options available to get yourself started with a microservices macro architecture. You would be extremely remiss to not consider the options as a part of your initial macro architecture conversations. Some of these existing infrastructure pieces make getting started quite easy — further supporting my stance that everybody can benefit from this.

Some of more cohesive off-the-shelf infrastructure projects are referred to as service meshes. Service meshes provide a control plane (clustered management of the service mesh proxies and other macro services) and a data plane (the proxy services through which your services communicate). They typically operate in the form of a sidecar proxy which provides the microservices networking functionality out of the box. Using one of these can give you a head start on the bulk of the functionality — and for many people, they may be more than you will ever need.

These projects are all relatively young, and they are going to impose limitations on your environment that you might not have otherwise chosen. However, they are designed and developed by people who know microservices very well, and you are permitted to both use their intelligence of what works and you can save a lot of time not recreating the technologies yourself.

Here are a few that I have found and done at least a moderate amount of investigation (these descriptions are surface-reading only — see the respective sites for more information!).

##### Netflix
Netflix is hot on the scene with microservices architecture, and they have open sourced much of their base run-time services and libraries. They work in the JVM, and include: Eureka for service discovery, Archaius for distributed configuration, Ribbon for resilient and intelligent inter-process and service communication, Hystrix for latency and fault tolerance at run time, and Prana as a sidecar for non-JVM based services.

The Netflix provided infrastructure pieces may be too big for a smaller shop, but if you are working in the JRE already, adding support for Eureka, Ribbon, and Hystrix can quickly grant you many benefits with potentially small amounts of investment.

##### Spring Cloud
Spring has long been a central place to go for frameworks enabling quick and easy JVM based software development. Their Spring Cloud specialized section includes integrations with a lot of cloud infrastructure, including the above mentioned Netflix libraries among many others. If you are going to go the JVM route, it will be worth your while to get to know Spring Cloud.

##### Linkerd (Service Mesh)
This service mesh, written by Buoyant, was released to the open source world early in 2016. It runs as a sidecar and acts as a proxy between your services provides you with: load balancing, circuit breaking, service discovery, dynamic request routing, HTTP proxy integration, retries and deadlines, TLS, transparent proxying, distributed tracing, and instrumentation. Protocol support includes HTTP/1.x, HTTP/2, gRPC, and anything TCP based.

Linkerd tries to not tie you down to any one technology — it supports running locally, in docker, in kubernetes, in DC/OS, in Amazon ECS, and more. As a sidecar application, it can be run once per service or once per host — so if you run multiple services per host, you can save on process overhead with Linkerd. They boast a couple of very well known names on their used by list. Interestingly, you can integrate Linkerd with Istio (covered below); I am unclear what the benefits of this is, but a surface reading says there may be something there.

##### Conduit (Service Mesh)
In December 2017, almost two years after Linkerd, Buoyant released another service mesh specifically for Kubernetes clusters. They took their lessons learned, and are creating Conduit with the intention of being any extremely light weight service mesh. The Conduit tooling works in tandem with the Kubernetes tooling to inject itself into your cluster. Once injected, most of the work happens behind the scenes through proxying and the use of standard Kubernetes service naming schemes. It claims good end-to-end visibility, but I do not see good screenshots of that, and have not yet tested it out myself.

A big caution here is the Alpha status and the extremely new creation — February, 2018. They have published a Roadmap to Production with insight of where they are going. For now I would test drive it and keep this one on the “to watch” list.

##### Istio (Service Mesh)
Istio is a service mesh which came to us in May of 2017. Internally they are using Envoy (covered next). They have instructions for deploying on top of Kubernetes, Nomad, Consul, and Eureka. As a sidecar, it provides automatic load balancing, fault injecting, traffic shaping, timeouts, circuit breaking, mirroring, and access controls for HTTP, gRPC, WebSocket, and TCP traffic. Ingress and Egress traffic is afforded the same feature set. Automatic metrics, logs, and traces are available quickly through included visualization tools. They also enable infrastructure level, run-time routing of messages based on content and meta information about the request.

Downside is it is very young and restricted to specific deployment environments — though there is some documentation that may help you deploy in other environments using manual methods. Istio uses iptables to transparently proxy network connections through the sidecar — in the Kubernetes world this is truly transparent to you, but in other environments you are involved in making that work. (It honestly looks like most of these mesh services are using iptables transparent proxy mechanisms to hook their sidecars into your applications.)

On the upside, the security feature set feels mature and well thought out. All egress connections are, by default, denied until explicitly permitted; that is refreshing! You protect your services within your mesh the same way you protect it at the ingress and Egress — nice! The out-of-the-box visualization of your services as a network diagram and various per-service metrics provides you immediate observability into your environment. Large scale deployments will likely need to own moving this into larger deployments, but as a getting started environment it is very nice.

##### Envoy (Data Plane)
Originally built by Lyft, but released after Linkerd in 2016, this one has the appearance of being the most mature. It boasts very large companies on its “Used By” list. It is written in C++ and is intended to be run as a sidecar like the rest. It was built to support running a single service or application as well as supporting a service mesh architecture. That said, Envoy is not a full service mesh as it only provides the data plane and you must manage the Envoy processes yourself or use Istio (which, by default, uses the Envoy proxy).

A quick look through the documentation shows a healthy list of features, including: filters, service discovery, health checking, load balancing, circuit breaking, rate limiting, TLS, statistics, tracing, logging, and much more. Connection type supported include HTTPS, TCP, and Websockets. I am impressed with Envoy from the window dressing, and given Istio’s use of Envoy I will most likely experience it through a test drive of Istio first and only look at Envoy alone if I feel there is something Istio is hiding or preventing me from utilizing fully.

##### Jump Start — Excitement Abounds!
I am extremely excited to sit down with each of these existing technologies and give them a thorough run-through. With the sheer amount of functionality they already provide, I would be woefully remiss to not understand them and include them as the basis for whatever microservices macro architecture I support in my organization.

Building all of this functionality from scratch, and not taking advantage of the great work already done by so many fine, brilliant individuals would be a crime. I would rather my organization spend its time on the services and functionality that makes them money — or, if we must extend more functionality to the macro architecture infrastructure, spend that time contributing back to one of these projects.

Utilizing one of these service meshes will require us to understand it extremely well. We must be able to discern the implications it has upon our macro architecture, and we must document those very carefully into our macro architecture. Oh yes, even if you choose a service mesh, you must still write down a macro architecture for your microservices infrastructure. These service meshes are only providing you an immense jump start, and, in some cases, answering some of the questions for you.

---

## Book Reference
[The Tao of Microservices](http://amzn.to/2pmifTF) - By Richard Rodger      
A great introduction to the world of microservices with a strong focus on the broad spectrum of requirements necessary to enter into this world. Richard starts with practical definitions and direction on how to build microservices followed by an overview of what it takes to run microservices. This book provides a good understanding of messages as transport, pattern matching for routing, and the large effort monitoring and measuring your environment will be. Warning: The author spends the first third of the book being rather derogatory towards any non-microservices approach to development. Read past that and he does have a good book.

[Microservices: Flexible Software Architecture](http://amzn.to/2FLmrH4) - By Eberhard Wolff.   
This book is broken up into logical sections. The first two give a lot of repetitive, background information on microservices presenting what they are, are not, and when you should and should not use them. There is a severe lack of commas in the book which sometimes trips you up, but the material is very good. Part 3 turned this book into a complete winner for me when he began covering very specific pieces of information.

[Building Microservices](http://amzn.to/2u16cQE) —   
This book is referenced by Martin Fowler and others as a key book in the movement.

[Production-Ready Microservices](http://amzn.to/2tZg5OP)—    
This book has been touted as one which dives directly into the topic of this article — the support a production platform should have to enable microservices.

[Domain Driven Design](http://amzn.to/2IxSOGQ) — 
Everybody in microservices is talking about this book. The Object Thinking book talked about this book. If everybody is talking about this book, it is highly likely one to ready.

[Building Microservices](https://amzn.to/2IU6Qmm):   
Designing Fine-Grained Systems by Sam Newman. Martin Fowler speaks very highly of this one. It purports to “provide lots of examples and practical advice.” I understand many of the principles; now I want to see more practical examples to further refine and firmly seat them.

[Production Ready Microservices](https://amzn.to/2IXBICD) by Susan J. Fowler.    
I believe Susan is going to drive more into this concept of a macro architecture for microservices. In this article, I have attempted to do in brief what I hope she will do in much more detail.