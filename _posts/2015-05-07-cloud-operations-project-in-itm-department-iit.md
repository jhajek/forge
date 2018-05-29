---
layout: post
status: publish
published: true
title: Cloud & Operations equipment in the ITM department IIT
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1363
wordpress_url: https://forge.sat.iit.edu/?p=1363
date: '2015-05-07 10:24:07 -0500'
date_gmt: '2015-05-07 15:24:07 -0500'
categories:
- Cloud
tags:
- cloud
- Eucalyptus
- Operations
comments: []
---
For a while I wanted to publish all of our research capabilities for the Cloud and Operations research group in the <a href="http://appliedtech.iit.edu/" title="School of Applied Technology">School of Applied Technology </a> <a href="http://appliedtech.iit.edu/itm" title="ITM">Information Technology and Management Program</a>.

[AppendixForITResroucesABET.pdf](assets/2015/05/AppendixForITResroucesABET.pdf "Appendix For IT Resources ABET")

### Eucalyptus 4.0.2 Cloud Computing Stack
Capacity is over 100 IPs instances supporting over 200 virtual machine instances over 4 nodes and up to 1 TB of Object based storage and 750 GB of EBS based storage.  Capabilities include loadbalancing and elastic deployment.  Systems are provisioned by Cobbler 2.4.1 instance running on Ubuntu 14.04 server edition.

Projects on the Eucalyptus Stack are currently running an elastic Hadoop Cluster with capacity up to 32 nodes.

Location IIT Rice Campus

*  Incheon - Dell PowerEdge 2970 12 cores 32 GB of memory 1 TB of 15K SCSI local storage
*  Alexandria - Dell PowerEdge R710 8 cores 96 GB of memory 3 TB of local storage  
*  Quantas - Dell PowerEdge R510 8 cores 32 GB of memory 3 TB of local storage 
*  Nebelet - Dell PowerEdge R510 8 cores 32 GB of memory 3 TB of local storage   
*  lexington.sat.iit.edu 1 Dell PowerEdge 1850
*  3 HP ProLiant DL360
*  oswego.sat.iit.edu -> cobbler server 1 Dell PowerEdge SC 1425

### Deployment and Operations Research Cluster
Provides research capabilities into CEPH distributed file system clusters and MAAS (Metal as a Service) clustering for application install.
16 Dell Poweredge 1425 40 GB harddrive 4 GB of memeory 2 Single core Xeon processors

#### Eucalyptus Private Test Cloud

Test Cloud 1 is used for the practice of installation and upgrade testing for production cloud, using Centos 6

*  1 WhiteBox servers 8 cores 16 GB memory 1 TB of local storage
*  3 ASUS RS100-X7 4 cores each 8 GB memory 1 TB local storage 

Test Cloud 2 is used for research into passthrough via VFIO in the Linux Kernel for use in on demand GPU cloud computing resources.**

*  1 WhiteBox servers 8 cores 16 GB memory 1 TB of local storage
*  3 ASUS RS100-X7 4 cores each 8 GB memory 1 TB local storage 
*  1 GPU cluster for GPU research Containing for AMD Radeon
*  3 white box storage systems.

(Visio Diagram to follow)
