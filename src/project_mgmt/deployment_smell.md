## Deployment Smells: The 5 Most Common Deployment Mistakes
[Original Source](https://www.enov8.com/blog/deployment-smells-5-most-common-deployment-mistakes/)
---

Deployment shouldn’t be the most nerve-racking task for IT and DevOps Teams, but many times it is.

If we’re deploying application changes to a beta system, we don’t care that much if the system goes down. Neither will your users—they know that beta means “expect problems from time to time.” The same thing applies to new systems that don’t have a large user base yet.

But what if we’re talking about bigger systems, like those used by a major online retailer or a bank? Users might get frustrated if they have problems shopping online. They may choose to buy from a different store as a result. And they’ll be furious if they aren’t able to access their bank account to make a withdrawal. Even if it’s just for a small fraction of time, companies lose credibility every time their systems are down or malfunctioning.

Deployments tend to get really difficult. Just how difficult is proportional to the complexity of the system you’re working with. The more dependencies the system has, the riskier and more problematic deployments get. But what are the common mistakes that we’ve all made?

#### 1 Not Enough Automation
Manual deployments **lack consistency**. So, when a significant portion of your procedures are manual, you wind up with a big ball of mud. I have suffered the consequences of doing deployments manually. I vividly remember one time when the site was down intermittently because I forgot to upload the binary files to 1/4 of the servers in the farm.

With the rise of DevOps, automation is becoming de facto. But don’t get overwhelmed if your process is still largely manual. You don’t need to stop what you’re doing and automate everything. I’ve always loved the boy scout rule to leave things better than you found them. It’s OK to start manually because it’ll help you understand how everything works. But do identify where automation can be used. You might want to be able to go from 0 to 100 to declare the task of automating your process “complete,” but **automation should be done progressively and continuously**.

The only manual thing you might want to have is someone approving the release, but even that should be a matter of just clicking a button. So, it’s not just about copying and pasting things; it’s also about configuration management, infrastructure, on-demand environments, and so on.

When you automate, you foster communication. If there’s only one way to make changes in deployment, everyone will know the what, why, and how of things. And if everyone is in the know, the guy or gal that solved a complex deployment problem can go on vacation without having their mai-tai-at-the-beach time interrupted by a clueless colleague back at the office.

#### 2 Different Environment, Different Way
I once fell into the trap of automating deployments by environment. In my case, there were four environments: dev, QA, staging, and production. I took my time to finish with the dev environment. Then I needed to replicate those changes in the other environments. The problem was that the deployment process for each environment was completely different in most cases.

I’m a big fan of the twelve-factor app methodology. Regarding deployments, it says that you need to keep config values in the environment, not in the application or any file. It also says that environments should be as similar as possible—not in size, but in style.

In other words, if you lack production-like environments, you’ll be lying when you say you’re ready to deploy to production. I’ve seen it, I’ve suffered it, and I stopped treating production as some sacred environment to be feared.

When I was part of a project where we Dockerized the system, I understood this very clearly. I used concepts like immutable infrastructure, infrastructure as code, environment configuration management, cloud, and deployment definition files with Kubernetes as pieces of the deployment puzzle to make things easier.

#### 3 Waiting for Low-Traffic Time Slots
Any time you hear (or say) “Let’s wait for the weekend to make this deployment,” it’s a sign the deployment pipeline isn’t trustworthy. You’re still treating production significantly differently from other environments.

Netflix and the other “unicorns” in the industry say they actually do deployments during regular work hours. After all, that’s when everyone is together and ready to react properly if there’s an incident. I know you might not be as big as the unicorns, but there’s nothing special about them aside from one thing: they experience problems at a really large scale. That forces them to change the way they’re doing things.

As I said before, you probably do deployments during low-traffic hours or over the weekends because you want to prevent any downtime in the system. But instead of living in fear of screwing things up, why don’t you set the foundation to experiment?

Do deployment strategies like feature flags, canary releases, blue/green deployments, or rolling updates ring a bell? Maybe, maybe not. Either way, these are great strategies that help you make deployments without users noticing it. And it’s a way to avoid stealing family time away from the staff.

#### 4 Waiting Until Enough Changes Accumulate
Do you have changes that are ready to be deployed? Why wait? To take one out of Nike’s book, just do it!

OK, so maybe something official is delaying you. Maybe you’re waiting for the change advisory board (CAB) to approve the changes based on the risk. I’ve been there too. But maybe you just need to improve communication by automating some steps in the process. These changes don’t come overnight—it takes time for the CAB to trust the process.

Another reason why organizations wait to do deployments until enough changes have been accumulated is, again, to reduce downtime. Downtime usually happens because you’re changing something. It rarely happens because of nightmarish things like the cloud provider going down, fires, earthquakes, and so on.

But I’ll say it again: don’t wait! Work to build a culture that deploys frequently and in small batches. If it hurts, do it more often. Every sprint, you can deploy something that can easily be disabled with a feature flag. Once you decide it’s ready to be used by everyone, turn the feature on to release the changes.

When you deploy too many things at once, you don’t know which of those things might be causing problems. And when you don’t know what’s causing problems, you have to roll back everything, including perfectly functional features that may have been really critical for the system.

#### 5 A Lack of Subsequent Feedback
You might have good monitoring and observability for your systems, but how sure are you that problems are being caused by a recent deployment and not by something else? There are lots of possible factors, such as internet problems, cloud provider issues, and an increase in traffic.

Another problem is that after a deployment, we tend to monitor changes for a while and then forget about it. But if you end up working with feature flags, it can take some time to activate or release a change. For that reason, it’s important that you have a smart way to correlate events, logs, and any other information you’ve found useful as you’ve learned the system’s behavior.

It’s also important that you have feedback after deployments because that’s how you’ll know if something needs to be rolled back. Without reliable feedback, any issues will just continue to affect your users. Let your team know when a deployment and release happen. It could be as simple as a Slack message in a channel. That way, people can “subscribe” to get notified, ask questions, and learn how the team solves problems.

#### In summation – Shift Deployments to the Left
In a nutshell, don’t treat production differently from your other environments. If you want to have a high rate of successful deployments, practice them (& standardize them) before going live. I’ve even been in the situation of announcing a release to the CAB for a certain date and then having to delay release dates because we found problems when we were launching the B side of our blue/green deployment.

I’ve also come to the conclusion that most of the things I’ve written here are more of a cultural change than anything else—a mindset that has to be adopted by the team that’s involved in building the software, not just developers and operations.

Start small and keep analyzing areas where you’re consistently struggling during deployment. Then improve. Use Enterprise Intelligence solutions like enov8 to continually understand your IT & Test Environments and your Release Management (inc Deployment Management) processes. The end goal should be that, at some point, you might leave releases to marketing or management because you already took care of doing deployments with almost no downtime.

And how do you get there? Practice until you’ve mastered deployments.