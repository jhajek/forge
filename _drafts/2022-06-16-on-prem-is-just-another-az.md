---
layout: post
status: publish
published: true
title: 'On-Prem is just another Cloud Availability Zone'
tags: 
- cloud
- on-prem
- AZ
- teaching
comments: []
---

## Why are you running on-prem?

The first question I receive when talking about the Cloud Lab is something akin to, *"Why not just use AWS?"* And this is a fair question. Having an on-prem Cloud Infrastructure has benefits over AWS or Azure or other cloud platforms.  

## On-prem teaching tool

For teaching this is a great tools to have.  When something goes wrong, you can open up the hood and get into the logs and search, troubleshoot, and learn. You can also let the students get into the guts of the system and let them learn.  This learning allows for asking deeper questions of the system--which is the essential piece of troubleshooting.

I define troubleshooting/debugging as not making the problem go away, but asking questions of a system until the answer reveals itself and you have gained a deeper fundamental understanding of your system.

### On-prem is just another Availability Zone (AZ)

There is also an argument that I feel is a false dichotomy, that is Public Cloud vs. Private Cloud. If one defines Cloud as simply elastic usage of someone else's excess computing then that comparison can exist. But that leads to the idea that only Public Cloud providers can do "Cloud Computing" and that the rest of us can't or are not doing it.  

Here is what I believe as the basic principles that define cloud:

* Services accessible via API over HTTP
* Elastic Block and File Storage
* Elastic Compute
* Elastic Object Storage
* Identity and access management

When you define Cloud in that Fashion, then on-prem can become just another AZ in your computing strategy.  That is why having your own on-prem cloud infrastructure is important -- not just for teaching.