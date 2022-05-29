---
layout: page
status: publish
published: true
title: Cloud Hadoop Operations Lab 
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
date: '2020-12-09 11:03:41 -0500'
categories: []
tags: []
comments: []
---
For a while I wanted to publish the research capabilities of Professor Jeremy Hajek for the Cloud and Operations research group in the [Information Technology and Management Program](http://iit.edu/itm "ITM") Program at the [College of Computing](https://www.iit.edu/computing "College of Computing").

## Current Hardware Platform

*Update 12/09/20*
Current applications include:

* 5 node [Hadoop 2.8.5](http://hadoop.apache.org/ "Hadoop Cluster") Research Cluster
  * 197 GB RAM and 47 processors available.
* 1 [Pfsense 2.4](https://www.pfsense.org/ "Pfsense") VPN router for access to the cluster
* 1 Instance of [Cobbler 2.8.x](https://cobbler.github.io/ "cobblerd") for network based installs
* Use of [Riemann](http://riemann.io/ "riemann") for metric and event routing
* 1 instance of [Grafana](https://grafana.com/ "grafana") for visual inspection of metrics
* 2 instances of [Docker](https://www.docker.com/ "docker") running on Ubuntu 16.04 LTS

### Current Hardware

* Namenode  - 1.8 TB of storage, 32 GB of RAM, [AMD FX 6100 Hexacore](https://en.wikipedia.org/wiki/List_of_AMD_FX_microprocessors#Bulldozer_Core_.28Zambezi.2C_32_nm.29 "AMD FX 6100")
* Datanode1 - 3.6 TB of storage, 94 GB of RAM, 2x [Intel Xeon E5530 Quad Core](https://ark.intel.com/products/37103/Intel-Xeon-Processor-E5530-8M-Cache-2_40-GHz-5_86-GTs-Intel-QPI "Interl E5530")
* Datanode2 - 2 TB of storage, 56 GB of RAM, 2x [Intel Xeon E5530 Quad Core](https://ark.intel.com/products/37103/Intel-Xeon-Processor-E5530-8M-Cache-2_40-GHz-5_86-GTs-Intel-QPI "Intel 5530")
* Datanode3 - 2 TB of storage, 32 GB of RAM, 2x [Intel Xeon E5504 Quad Core](https://ark.intel.com/products/40711/Intel-Xeon-Processor-E5504-4M-Cache-2_00-GHz-4_80-GTs-Intel-QPI "Intel 5504")
* Datanode4 - 2 TB of storage, 24 GB of RAM, [AMD FX 6100 Hexacore](https://en.wikipedia.org/wiki/List_of_AMD_FX_microprocessors#Bulldozer_Core_.28Zambezi.2C_32_nm.29 "AMD FX 6100")
* Datanode5 - 2 TB of storage, 32 GB of RAM, [Intel Xeon E5530 Quad Core](https://ark.intel.com/products/37103/Intel-Xeon-Processor-E5530-8M-Cache-2_40-GHz-5_86-GTs-Intel-QPI "Interl E5530")

### Software Support

* Supported Software
* MapReduce
* Spark
* SparkR
* SparkQL
  
<p>Actual Image Located at the Wheaton Rice Campus - Room 242<br />
<a href="https://forge.sat.iit.edu/wp-content/uploads/2015/08/WP_20150506_12_32_34_Pro.jpg"><img src="https://forge.sat.iit.edu/wp-content/uploads/2015/08/WP_20150506_12_32_34_Pro-169x300.jpg" alt="WP_20150506_12_32_34_Pro" width="169" height="300" class="aligncenter size-medium wp-image-1934" /></a></p>
