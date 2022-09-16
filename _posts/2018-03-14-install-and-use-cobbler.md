---
layout: post
status: publish
published: true
title: 'Install and Use Cobbler 2.8 on Centos 7.4'
author:
  display_name: Jeremy Hajek
  login: jhajek
  email: hajek@iit.edu
date: '2018-03-14 06:39:52 -0600'
date_gmt: '2018-03-14 12:09:52 -0600'
tags: 
- Cluster Cobbler Centos VPN Research
comments: []
---

## Why is Network Install Important?

Over the years I have looked at other bare metal installation programs.

* Solaris and Kickstart
* Clonezilla
* RedHat Satellite
* Ubuntu MAAS
* Windows SuS
* [Cobblerd](http://cobbler.github.io/about.html "cobblerd")

A few factors guided my desires.  I work at a university so the internal network is blazing fast--as well as the external network compared to home internet here in America.  

Also as a researcher I found that I was constantly exploring and experimenting with various software configurations.  If we go back in time, there was a point where a single CD-ROM or a single flash drive and an install would suffice. But looking at modern software implementations, this is no longer possible or justifiable.   Not only for IT staff or Professors, but students should have access to this kind of laboratory.  Software is complex and should not be a fragile operation.

Out of all of the items listed above, I had found that Cobbler was the only one that worked.  Though Cobbler is old and shows signs of a by-gone era, it gets the job done for me.  Its simple enough and the network based installs are reliable that it has shaved days off of various tasks.

Cobbler with a simple series of commands, network installs can be configured for PXE, reinstallations, media-based net-installs, and virtualized installs (supporting Xen, qemu, KVM, and some variants of VMware). I would like to revisit Canonical MAAS now that Ubuntu 18.04 is available as well.
