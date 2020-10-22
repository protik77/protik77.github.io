---
layout: post
author: Protik Das
title:  "Mindset of designing an air-gapped systems"
date:   2020-10-22
---

> This post is a work in progress where I am trying to write down the mindset that is needed to design an air-gapped HPC system.

We are so used to everything connected to internet that, the limitations of an air-gapped system might seem alien at first. The company I work for, KLA (earlier known as KLA-Tencor) have been building air-gapped systems for decades. In fact KLA is one of the first few semiconductor focused companies that were establibshed in Silicon Valley. My experience in this domain is novice at best. Like everything else, I try to capture the principles that drive a concept. So here's my first stab about the mindset that is needed to design an air-gapped system. While I myself work on such HPC systems, I will try to keep it general enough for other domains as well.

### Anything that can go wrong, will go wrong

If you have worked with system administration for a while, you must be familiar with Murphy's law by now. Have a backup of the data, have some spares in case of a hardware failure and we will see, right? Not so fast for an air-gapped system. How do we know it's a software problem or a hardware problem? If it's a hardware problem, which component is the culprit? Being an air-gapped system, we can't just do `apt install magical-debug-tool` and find out what's happening.

So we have to think couple of steps ahead. We have to list down possible scenarios during the design phase where the system will break. The objective would be to include the necessary tools there. We also have to test them before we ship the system. We don't want to add a script that might fail when the system is down.

It's imperative that we can't predict all the possible scenarios. What if our existing tools and scripts doesn't cover a scenario? So what we can do is to have a comprehensive log collection system for further investigation. In a scenario where we don't have a tool to debug the problem, last resort is to have all the logs.

### Everything needs to be version controlled

It's easy to keep things version controlled when it's code. What about the operating system? Can we version control an OS that we are customizing from ground up? We can use confiuration management systems to maintain a "state". But how about having a state at the first place? Also inherently it is assumed that state management systems will have access to internet which isn't true anymore. We can work around that, but more than often that's not ideal.

While there is terraform like solutions for cloud based systems, the bare metal world is heavily platform dependent. We also have some requirements that limit our options. We got lucky and came across [kiwi](https://osinside.github.io/kiwi/). This is developed by nice folks of Suse and works excellent for rpm based Linux systems. Kiwi gives us a very fine tuned way of building a custom OS that is also version controlled. How cool is that? If you are curious, check out this github repo to check out some minimal working versions of different OS versions: [kiwi descriptions](https://github.com/OSInside/kiwi-descriptions).

Thanks to kiwi, when we have to create an OS for a new system, they are all version controlled and we know exactly what is included there.

### Any task that is automated, needs to have a manual way to achieve the same

Automation is great. We all love automation. But often when we automate something, some certain scenarios are assumed. When these scenarios are different, it's quite hard to adopt anything new in an air-gapped system.

Story time. When powering on a system for the first time in an air-gapped system, we chose to mount our object storage from server 1 of 3 available servers. We wrote a script, it was working great in our in-house system. Now when the system is far from home in an air-gapped system, something new happened. There was a power trip when the servers were being turned on. "Something" happened with server 1 and it isn't turning on. No worries, we have 2 more servers, right? Yes. But the script we shipped only mounts from server 1 when being powered on for the first time. Who would have thought this might happen the very first time? Not me. Now because server 1 is down, we can't make the script work increasing down time. And not to mention cascading affects that come with it.

The upshot is, assumptions happen. It's impossible to capture all the possible scenarios, there are simply too many variables. For our case, the mistake was too much automation. We automated the process, but didn't have a way to achieve the same manually. Having a manual way to do the same could help us power up the system way faster reducing down time. So while automating is great, there must me a way to achieve the task manually as well.

### Keep dependency on remote resources minimal, zero if possible

The systems we design has a quite long shelf life. Think a decade or so. And that brings in a new set of challenges.

Software release cycles are now faster than ever. There are software out there that goes through very fast release cycles. Maybe a software that we use directly doesn't have a faster release cycle, but a dependency may have. Let's say we are 5 years into the lifecycle of a system and we need to reproduce an OS image that we created 5 years ago. We have even version controlled the OS. But often the problem is developers and vendors move on to the new versions and stops supporting older versions. Not only that, remote links become obsolete, packages and softwares disappear. Yes, just like that. Simply a 404 error and we can't reproduce the OS. Recently docker [decided](https://www.docker.com/pricing/resource-consumption-updates) to get rid of images that didn't have a pull in last 6 months. Imagine coming back after years and finding that the image doesn't exist.

The solution to this is to have a mirrored version of the repositories or packages in-house. It's not easy as it sounds. It also needs significant investment in the infrastructure and someone has to manage them. But it's worth every penny. Also the vendors or developers that work with enterprise are familiar with in-prem solutions, but more than often there's someone who is not.

In essence, dependency on anything remote should be kept minimal. It's absolute best if there's no dependency on remote resource at all. Be it a software repository or a single executable.
