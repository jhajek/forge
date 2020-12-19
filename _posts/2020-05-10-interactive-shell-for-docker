---
layout: post
status: publish
published: true
title: 'Interactive Shell for Ubuntu Bionic Docker Containers'
date: '2020-05-10 20:55:52 -0600'
date_gmt: '2020-05-10 17:55:52 -0600'
categories:
- software
- containers
tags: 
- docker
- containers
- paradigms
- abstraction
- Ubuntu
comments: []
---

In the process of understanding the Docker OS Container paradigm, you sometimes need to have a remote shell into a Docker container.  In this case I was trying to run multiple services on top of a Docker Ubuntu:Bionic container.   The services would start and exit properly, when instead I wanted them to run, like a daemon.  Init systems like sysVinit and systemd don't exist in a container because that is the wrong way to think about containers and the abstraction.  

Here is the command that I could get a remote shell into a Docker Ubuntu Bionic container so I could install the packages and run the commands I was trying to run and see what was happening. Then once I solved the problem I could go back and automate this solution.

```bash

sudo docker run -it ubuntu:bionic
```

