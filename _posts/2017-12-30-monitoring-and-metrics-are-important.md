---
layout: post
status: publish
published: true
title: MVP Minimal Viable Platform or why monitoring and metrics are essential  
date: '2017-12-30 12:11:43 -0500'
date_gmt: '2017-12-30 20:11:43 -0500'
tags:
- IllinoisTech
- SmartTechLab
- riemann
- graphite
- deployment
- metrics
comments: []
---

In the everexpanding IT and cloud-based world, there are massive productivity gains.  There are also unforseen complexities introduced.   At one time, a single person could administer a handful fo servers.  This is why the profession was called *system administration*, because you had basically one system to administer.  

On this blog alone, we have seen the mention of a hadoop cluster, [Cobbler](https://en.wikipedia.org/wiki/Cobbler_\(software\)), and [Docker clusters](https://www.docker.com/). All of these platforms need to be managed.  How to manage them?  

This brings about the concept of MVP - Minimal Viable Platform.  I am not sure if the phrase originated with him, but Casey West is a Principal Technologist focused on Pivotal’s Cloud Foundry Platform.  At his [2016 DevOps Days London presentation](https://www.devopsdays.org/events/2016-london/program/casey-west/) he gave a great presentation, with the idea that monitoring and metrics of an application is **VITAL**--necessary before an application can be pushed out the door.  It should be part of the basic set up of the application. 

[John Allspaw](https://technical.ly/brooklyn/2017/11/14/former-etsy-cto-john-allspaw-launching-new-company/), said, *"If you code it, you should have a way to monitor and metric it."*  His point is, your buiness depends on these features, and if you don't know what is happening to your systems, better yet if you cannot generate a baseline of what is healthy for your application, then I hope you are a prayin' person because that is all you have.  If you are depending on your customers to tell you there is an outage, or worse random slowdowns, you will probably either be fighting fires constantly (and not innovating) or out of business.

So where am I going with this?  In the process of launching a 4 node Hadoop Cluster, supporting 50+ student with their assignments and projects.  12 TB, 146 GB of RAM, and 46 Cores makes this almost impossible for 1 person to monitor.  

In comes this book from James Turnbull, the [Art of Monitoring](https://artofmonitoring.com/) from [Turnbull Press](https://turnbull.press/).   The book outlines various tools and solutions to monitoring your applciaitons in the correct way.  ![*Art of Monitoring*](/assets/2017/12/aom.jpg)

Long had we done pull metrics, sampling of systems in periods, say 5 seconds, 30 seconds, or every 5 minutes.  But what happens when technology such as Virtualization or Containers allows you to create and deploy entire systems for a purpose, then destroy them all within the 5 minute monitoring period?  All of a sudden pull metrics give you a dangerously inacurrate picture.  

The capability and need has arrisen for **PUSH** metric and log collection.  Our metrics should be pushed from all of our *nodes* or any system that we launch or create--by default.  They should be automated (Cobber) when deployed they are pre-configured to immediately begin to communicate with an *event router*.  

What is an *event router*? It is a system that receives, logs, metrics, and events from a system and based on criteria you set will route those events, whether that is to a notification system, a graphing solution like Graphite, to email or text, or a log aggregation systems like the [ELK stack](https://www.elastic.co/products).  The event router of choice (and I know there are many) that I was introduced to is [Riemann](https://riemann.io). Riemann provides low-latency, transient shared state for systems with many moving parts.  Riemann demo video here:

{% vimeo 131385889 %}

Riemann is used to collect all of the metrics and logs pushed to it, filter them as I see fit and send them to a graphing solution.  What sort of metrics am I collecting for a Hadoop cluster? I am using [collectd](https://collectd.org/) which is a trusted solution for collecting the traditional CPU, RAM, and HDD usage stats.  These and other metrics are being collected and filter through riemann and graph them with Graphite and Grafana. 

![*Grafana](/assets/2017/12/graphite.png)

[Grafana](https://grafana.com/) is an industry standard tool that provides a graphing framework based on data you feed to it.

This is the core of our MVP (Minimal Viable Platform) in managing our Hadoop Cluster.  I will give the specifics in the next post.