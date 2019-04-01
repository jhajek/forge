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
date: '2015-08-22 16:35:41 -0500'
date_gmt: '2015-08-22 21:35:41 -0500'
categories: []
tags: []
comments: []
---
For a while I wanted to publish the research capabilities of Professor Jeremy Hajek for the Cloud and Operations research group in the [Information Technology and Management Program](http://appliedtech.iit.edu/itm "ITM") Program at the [School of Applied Technology](http://appliedtech.iit.edu "School of Applied Technology"). 

## Current Hardware Platform - as of 10/26/17

Current applications include:

* ~~4 node [Hadoop 2.6.5](http://hadoop.apache.org/ "Hadoop Cluster") Research Cluster~~
* 5 node [Hadoop 2.8.5](http://hadoop.apache.org/ "Hadoop Cluster") Research Cluster
    + 197 GB RAM and 47 processors available.
* 1 [Pfsense 2.4](https://www.pfsense.org/ "Pfsense") VPN router for access to the cluster
* 1 Instance of [Cobbler 2.8.x](https://cobbler.github.io/ "cobblerd") for network based installs
* Use of [Riemann](http://riemann.io/ "riemann") for metric and event routing
* 1 instance of [Grafana](https://grafana.com/ "grafana") for visual inspection of metrics
* 2 instances of [Docker](https://www.docker.com/ "docker") running on Ubuntu 16.04 LTS

### Current Hardware

* Hadoop Nodes
    + Namenode  - 1.8 TB of storage, 32 GB of RAM, [AMD FX 6100 Hexacore](https://en.wikipedia.org/wiki/List_of_AMD_FX_microprocessors#Bulldozer_Core_.28Zambezi.2C_32_nm.29 "AMD FX 6100")
    + Datanode1 - 3.6 TB of storage, 94 GB of RAM, 2x [Intel Xeon E5530 Quad Core](https://ark.intel.com/products/37103/Intel-Xeon-Processor-E5530-8M-Cache-2_40-GHz-5_86-GTs-Intel-QPI "Interl E5530")
    + Datanode2 - 2 TB of storage, 56 GB of RAM, 2x [Intel Xeon E5530 Quad Core](https://ark.intel.com/products/37103/Intel-Xeon-Processor-E5530-8M-Cache-2_40-GHz-5_86-GTs-Intel-QPI "Intel 5530")
    + Datanode3 - 2 TB of storage, 32 GB of RAM, 2x [Intel Xeon E5504 Quad Core](https://ark.intel.com/products/40711/Intel-Xeon-Processor-E5504-4M-Cache-2_00-GHz-4_80-GTs-Intel-QPI "Intel 5504")
    + Datanode4 - 2 TB of storage, 24 GB of RAM, [AMD FX 6100 Hexacore](https://en.wikipedia.org/wiki/List_of_AMD_FX_microprocessors#Bulldozer_Core_.28Zambezi.2C_32_nm.29 "AMD FX 6100")
    + Datanode5 - 2 TB of storage, 32 GB of RAM, [Intel Xeon E5530 Quad Core](https://ark.intel.com/products/37103/Intel-Xeon-Processor-E5530-8M-Cache-2_40-GHz-5_86-GTs-Intel-QPI "Interl E5530")

### Software Support

* Supported Software
  + MapReduce
  + Spark
  + SparkR
  + SparkQL
  
## Decommissioned

[AppendixForITResroucesABET.pdf from 05/15](https://forge.sat.iit.edu/wp-content/uploads/2015/05/ "ABET Appendix")

### HP Eucalyptus 4.0.2 Cloud Computing Stack
<strong>Updated: 08/10/2016</strong><br />
<strong>Updated: 12/02/2016</strong></p>
<p>Eucalyptus is a private cloud platform now owned by <a href="http://www8.hp.com/us/en/cloud/helion-eucalyptus.html">HPE</a>.  Similar products would include <a href="https://www.openstack.org/">OpenStack</a>.  Eucalyptus made the choice to make itself <a href="https://www.openstack.org/">Amazon Web Services</a>compatible.  Though they lag behind in some cases, but the core functionality and syntax makes this still a useful product.</p>
<p>Our capacity is over 100 IPs supporting potentially over 200 virtual machine instances.  There are currently 4 node controllers (using Linux KVM), <del datetime="2016-12-02T16:58:56+00:00">1 TB of object based storage </del>(Object Storage Gateway) 500 GB of object based storage and <del datetime="2016-12-02T16:58:56+00:00">750 GB of Elastic Block based storage (EBS)</del> 11 TB of Elastic Block based storage (EBS) using <a href="https://www.cdw.com/shop/products/WD-Red-WD30EFRX-hard-drive-3-TB-SATA-600/2764461.aspx">Western Digital Red</a> drives.  The HP Eucalyptus cloud supports similar function and control as AWS cloud.   Capabilities include:</p>
<ul>
<li>Elastic Load-balancing</li>
<li>Security Groups (Firewall)</li>
<li>RSA key based authentication (no passwords ever)</li>
<li>Cloud-watch (internal metrics)</li>
<li>Auto-scaling groups</li>
<li><del datetime="2016-12-02T16:58:56+00:00">University VPN access has been configured and used</del></li><br />
</ul></p>
<p> There is a matrix of AWS API support as well that I configured and published:</p>
<ul>
<li><a href="https://forge.sat.iit.edu/2016/06/eucalyptus-4-0-2-supports-using-walrus-aws-java-sdk-1-11-4-but-really-1-9-4/">Eucalyptus 4.0.2 supports using AWS Java SDK 1.11.4 for Walrus but really 1.9.4</a></li>
<li><a href="https://forge.sat.iit.edu/2016/03/aws-for-php-3-x-on-eucalyptus-4-0-2-works-like-a-charm/">AWS SDK for PHP 3.x on Eucalyptus 4.0.2 Works Like a Charm</a></li>
<li><a href="https://forge.sat.iit.edu/2016/05/using-s3cmd-with-walrus-and-hp-eucalyptus-4-0-2-works-but-with-a-twist/">Using S3cmd with Walrus on Eucalyptus 4.0.2 works but with a twist.</a></li>
<li><a href="https://forge.sat.iit.edu/2016/02/euca2ools-3-1-x-installation-and-connection-testing-to-eucalyptus-4-0-2/">Euca2ools 3.1.x installation and connection testing</a></li><br />
        </ul></p>
<p>There are two choices of instances that are currently available - Centos 6.8 and Ubuntu 14.04.5</p>
<p>The most recent course to run using this system was ITMT 430 Undergraduate Capstone Course which spent a semester building and deploying salable applications. Services are self-serve via a portal <a href="https://eucalyptus.sat.iit.edu">https://eucalyptus.sat.iit.edu</a> or via command line tool kit called Euca2ools.   Current status requires a username password to be created for access - Active Directory integration is available and I would like to pursue this over the winter break of 2016 to have ready for Spring of 2017 and expand the availability to a larger group.</p>
<p>Location IIT Rice Campus</p>
<ul>
<li>Incheon - Dell PowerEdge 2970 12 cores 32 GB of memory 1 TB of 15K SCSI local storage</li>
<li>Alexandria - Dell PowerEdge R710 8 cores 96 GB of memory 3 TB of local storage  </li>
<li>Quantas - Dell PowerEdge R510 8 cores 32 GB of memory 3 TB of local storage </li>
<li>Nebelet - Dell PowerEdge R510 8 cores 32 GB of memory 3 TB of local storage   </li>
<li>eucalyptus.sat.iit.edu 1 Dell PowerEdge 1850</li>
<li>White Box Cloud Controller Intel G850 Processor 16 GB of memory 11 TB of storage</li>
<li><del datetime="2016-12-02T16:58:56+00:00">oswego.sat.iit.edu -> Cobbler server 1 Dell PowerEdge SC 1425</del></li><br />
</ul></p>
<p><strong>Deployment and Operations Research Cluster</strong></p>
<p>Provides research capabilities into CEPH distributed file system clusters and MAAS (Metal as a Service) clustering for application install.</p>
<p>16 Dell Poweredge 1425 40 GB hard drive 4 GB of memory 2 Single core Xeon processors</p>
<p><strong>Eucalyptus Private Test Cloud<br />
</strong><br />
Test Cloud 1 is used for the practice of installation and upgrade testing for production cloud, using Centos 6</p>
<ul>
<li>1 WhiteBox servers 8 cores 16 GB memory 1 TB of local storage</li>
<li>3 ASUS RS100-X7 4 cores each &Gamma;&Ccedil;&ocirc; 8 GB memory 1 TB local storage </li><br />
</ul></p>
<p><strong>Test Cloud 2 is used for research into pass-through via VFIO in the Linux Kernel for use in on demand GPU cloud computing resources.</strong></p>
<ul>
<li>1 White Box servers 8 cores 16 GB memory 1 TB of local storage</li>
<li>3 ASUS RS100-X7 4 cores each &Gamma;&Ccedil;&ocirc; 8 GB memory 1 TB local storage </li>
<li>1 GPU cluster for GPU research &Gamma;&Ccedil;&ocirc; Containing for AMD Radeon</li>
<li>3 white box storage systems.</li><br />
</ul></p>
<p>Actual Image Located at the Wheaton Rice Campus - Room 242<br />
<a href="https://forge.sat.iit.edu/wp-content/uploads/2015/08/WP_20150506_12_32_34_Pro.jpg"><img src="https://forge.sat.iit.edu/wp-content/uploads/2015/08/WP_20150506_12_32_34_Pro-169x300.jpg" alt="WP_20150506_12_32_34_Pro" width="169" height="300" class="aligncenter size-medium wp-image-1934" /></a></p>
