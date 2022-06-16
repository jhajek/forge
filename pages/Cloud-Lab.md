---
layout: page
status: publish
published: true
title: Cloud Lab
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
author_login: hajek
author_email: hajek@iit.edu
date: '2020-12-09 11:03:41 -0500'
categories: []
tags: []
comments: []
---
For a while I wanted to publish the research capabilities of Cloud Lab research group in the [Information Technology and Management Program](http://iit.edu/itm "ITM") Program at the [College of Computing](https://www.iit.edu/computing "College of Computing").

## Hardware Platform

* Dell PowerEdge R730
  * Used as a remote-shared build-server for building and deploying VMs
  * 2 x E5-2620 v3 @ 2.40GHz
  * 143 GB RAM
* HP DL 380 G7 x2
  * 24 x Intel(R) Xeon(R) CPU X5690 @ 3.47GHz
  * 143 GB RAM
* Fujitsu TX300 S7
  * 24 x Intel(R) Xeon(R) CPU E5-2620 @ 2.00GHz
  * 188 GB RAM

## Opensource Software for the on-prem Cloud

So what can one use for on-prem cloud?  

* Version Control for Configurations
  * [Git](https://git-scm.org "Git website")
  * [Github](https://github.com "GitHub website")
* Virtualization Mangement (Elastic Compute)
  * [Proxmox 7](https://proxmox.com "Proxmox website")
* S3-Compatible Object Storage
  * [Min.io](https://min.io "Minio webpage")
* DNS forwarding
  * [Consul](https://consul.io "Consul website")
  * Cloud images are not static but ephemeral
* Virtual Machine Template Construction
  * [Packer](https://packer.io "Pacjer website")
* Virtual Machine Infrastructure Deployment
  * [Terraform](https://terraform.io "Terraform website")
* Password Managent
  * All public private keys and API tokens -- no passwords
  * [Twisted Ewards Curve](https://en.wikipedia.org/wiki/Twisted_Edwards_curve "Twisted Edwards Curve wiki page")
* Monitoring
  * [Grafana](https://grafana.com/ "Grafana website")
* Event Routing
  * [Riemann](http://riemann.com "Riemann Website")
* Metrics Collection and Storage
  * [Telegraf](https://www.influxdata.com/time-series-platform/telegraf/ "Telegraf webpage")
  * [Graphite](https://graphite.org "Graphite webpage")
* Direct Attached Storage
  * [TrueNas](https://truenas.com "Truenas website")
  * [iSCSI](https://en.wikipedia.org/wiki/ISCSI "iSCSI wiki page")

### Conclusion

There are many software options--if your software is left off the list its not a detriment to you.
