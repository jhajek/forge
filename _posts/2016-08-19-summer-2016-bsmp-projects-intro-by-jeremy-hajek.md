---
layout: post
status: publish
published: true
title: Summer 2016 BSMP IoT Prototype Projects - Intro by Professor Jeremy Hajek
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
date: '2016-08-19 23:33:54 -0500'
date_gmt: '2016-08-20 04:33:54 -0500'
tags:
- IoT
- presentation
- Smart Lab
- presentations
- bsmp
comments: []
---
{% youtube fm8YQOf28mA %}
The group from Brazil did an excellent job this summer on over 11 different projects ranging from cyber-security to IoT prototypes.  

Let me introduce the projects:

**[Autonomous Movement Framework for UAVs (uAMF)](/amf/2016/09/28/autonomous-movement-framework-presentation-at-uniforum.html "AMF")**
This project successfully built and deployed hardware and software to manage the autonomous movement of UAVs based on GPS location.  The goal was to be able to call a carrier drone to your location via a standard smart phone app and have a central server process the order, queue the order up, and distribute the order to a waiting drone.  This was done using Pixhawk processor and 3DR Iris copters.

**[Lamp Sense](/projects/presentation/2016/08/18/lamp-sense-using-intelligent-lighting-for-two-way-communication-bsmp-summer-2016.html "Lamp Sense")**
This project involves using the [Philips Hue](https://www2.meethue.com/en-us "Hue Lights") Intelligent lighting platform to give lighting a 2 way purpose -- to allow lights to be able to direct a person to a specific location.

**[Bluetooth Beacon](/projects/presentation/2016/08/18/bluetooth-beacons-bsmp-summer-2016.html "Bluetooth Beacon")**
This project involved creating and utilizing Bluetooth LE in order to create intelligent beacons--useful for triggering location awareness in mobile applications. 

**[Project S.L.A.M.- Solar based Location Awareness Module](/projects/presentation/2016/08/18/project-s-l-a-m-solar-location-awareness-module-bsmp-summer-2016.html "Project SLAM")**
This project solves a problem of creating a sensor that will warn people in a perpendicular hallway, when a heavy door is about to open into the hallway.  This project uses a sonic range finder and an Arduino Micro to handle the calculations.  The system is also battery backed and solar powered. 

**[Project Glass Charts](/projects/presentation/2016/08/17/project-glass-charts-google-glass-huds-summer-2016-bsmp.html "project Glass Charts")**
This project is using two pair of Google Glass and Android Chart Libraries in order to create real-time and voice activated graphs based on collected building HVAC data.  The purpose is to give field engineers a tool that can help them supplement conditions and make informed decisions. 

**[Project BatPack](/projects/presentation/2016/08/18/project-batpack-solar-powered-tactile-augmentation-for-visual-impairment-summer-2016-bsmp.html "Batpak")**
This project will create solar power and battery backed apparel that supplements a user&Gamma;&Ccedil;&Ouml;s rear and peripheral vision.  This is done by using sonic range finders and an algorithm for determining approaching bodies.  The apparel and computers are small enough as to be embedded in normal clothing and items and the battery combined with solar panels allow for a full days use off the grid.    

**[Project Manhunter](/projects/presentation/2016/08/18/project-manhunter-side-channel-leakage-abuse-attack-bsmp-summer-2016.html "Project Manhunter")**
This project is working on implementing research to test the hypothesis of leakage abuse attacks against searchable encrypted data.   This will be done using an object based storage (S3 based) product called Walrus from the HP Eucalyptus private cloud software. The goal is to be able to identify commonly encrypted data stored on this cloud by looking at side channel leakage.  Source: [Link to Dr. Perry's research](http://www.cs.lewisu.edu/~perryjn/ccs15.pdf "CCS15") 

**[Project Guest Pass](/projects/presentation/2016/08/11/project-guest-pass-gpu-passthrough-in-linux-kvm-summer-2016-bsmp.html "project Guest Pass")**
This system is investigating the ability to use GPU/PCI passthrough with Linux KVM to create self-service on-demand GPU clusters.  They will be using commodity x86 processors and AMD Radeon GPU cards
running on Linux using the KVM virtualization platform.   

**[Project Pykkon](/projects/presentation/2016/08/11/project-pykkon-summer-bsmp-2016.html "Project Pykkon")**
This project is focused on creating a man-in-the middle attack on an iSCSI connection stream--with the goal being to be able to inject or remove data from a known file without disrupting the TCP connection. 

**[Project Cyber Ghost](/projects/presentation/2016/08/17/project-cyber-ghost-using-dns-as-malware-cc-summer-2016-bsmp.html "Project Cyber Ghost")**
This project is researching the concept of using the DNS protocol and obfuscated DNS server entries as a command and control mechanism to send C&C malware traffic. 

**[Project Window Latch](projects/presentation/2016/08/10/project-window-latch.html "Window Latch")**
This project involves implementing confidentiality, integrity, non-repudiation and availability on the standard Arduino Uno platform -- ensuring that the data that was collected is the data that was sent. 
