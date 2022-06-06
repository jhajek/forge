---
layout: post
status: publish
published: true
title: Ethernet not autodetected in Ubuntu Preseed - auto install aborts
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
date: '2015-06-22 23:44:18 -0500'
date_gmt: '2015-06-23 04:44:18 -0500'
tags:
- cobbler
comments: []
---
When trying to use Cobbler 2.4.1 to install Ubuntu on some older Dell Poweredge 1425s. The strange thing is that the pressed script always drops to manual on the first step of detecting the network card.   You can manually select the right one and proceed with the install perfectly.  But this is frustrating as it breaks the entire idea of "automation."

I found the syntax to the answer here:  Though its tangential to the questions:
[http://serverfault.com/questions/143296/how-to-get-http-preseed-to-work-correctly-on-ubuntu-10-04-lts-lucid](http://serverfault.com/questions/143296/how-to-get-http-preseed-to-work-correctly-on-ubuntu-10-04-lts-lucid "Preseed on Lucid")

Look in the answer for the line ```interface=auto```. Here is where you need to add a kernel option or parameter to include interface=eth0 and everything will work.
