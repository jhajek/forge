---
layout: post
status: publish
published: true
title: Quad boot your PC - Ubuntu 15.04, Centos 6.6, Fedora 21, Windows 10
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
date: '2015-05-31 22:51:47 -0500'
date_gmt: '2015-06-01 03:51:47 -0500'
tags:
- boot
comments: []
---
## Quad Boot

In the course of working in the field I have come across the need to run 4 operating systems on my second laptop.

### Ubuntu

<a href="/assets/Ubuntu-Logo.jpg"><img src="/assets/Ubuntu-Logo-150x150.jpg" alt="Ubuntu-Logo" width="150" height="150" class="alignnone size-thumbnail wp-image-1395" /></a>

My heart is with Ubuntu ever since 7.04

### Fedora

<a href="/assets/fedora-logo.png"><img src="/assets/fedora-logo-150x150.png" alt="fedora-logo" width="150" height="150" class="alignnone size-thumbnail wp-image-1392" /></a>
Fedora is what I am curious of because they are always bringing in new technology - also this is used in textbooks to teach classes in school

### CentOS

<a href="/assets/centos_logo.png"><img src="/assets/centos_logo-150x115.png" alt="centos_logo" width="150" height="115" class="alignnone size-thumbnail wp-image-1391" /></a>

I personally don't like to use Centos as a desktop OS - because it is not really a desktop OS - but the Eucalyptus cloud platform runs on Centos 6.6 so all of my testing and image development needs to be on Centos 6.6

### Windows

<a href="/assets/windows-logo.png"><img src="/assets/windows-logo-150x150.png" alt="windows-logo" width="150" height="150" class="alignnone size-thumbnail wp-image-1394" /></a>

Running Windows 10 Tech preview here - seeing really great things running it natively since October of 2014 (no virtual machines here!)
What I did was install Ubuntu, then Fedora, then Centos.  I made sure to watch out and not use all the primary partitions (remember those... primary, extended, and logical?)  You have 4 primary partitions - but you have infinite logical partitions on the extended partition.   You can install Linux on a logical partition - make sure you do.  
Note you don't have to make a swap for each partition.  Just make one and reuse it across OSes.  Also I opted for traditional install not LVM allthough I am sure you could do this under LVM.   Each cascading operating system's boot loader will handle the previous one.  The trick become Windows 10.  I installed this last.  Usually the traditional wisdom is to do this first.  But Windows boot loader BCE has come a long way from XP days.

### Windows BCD

You can use a neat tool called <a href="http://neosmart.net/EasyBCD/">EasyBCD</a>  When you install Windows 10 and then go to the command line and launch <a href="https://technet.microsoft.com/en-us/library/cc709667(v=ws.10).aspx">BCDedit</a> when you list all the items you will only find Windows 10--eeep!  It looks like you just installed over your hard Linux work.
Relax you didn't. EasyBCD app (free for non-commercial student use) tool will find the partitions.  When you see the list choose the bootloader partition of the last Linux you installed.  That means you need to pay attention on which partition of the disk you installed the last Linux bootloader.  Then it works =).
