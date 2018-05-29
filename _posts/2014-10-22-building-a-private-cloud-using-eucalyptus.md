---
layout: post
status: publish
published: true
title: Building a private cloud using Eucalyptus
author:
  display_name: Sai Pramod
  login: supadyay
  email: supadyay@hawk.iit.edu
  url: ''
author_login: supadyay
author_email: supadyay@hawk.iit.edu
wordpress_id: 1190
wordpress_url: https://blog.sat.iit.edu/?p=1190
date: '2014-10-22 18:37:28 -0500'
date_gmt: '2014-10-22 23:37:28 -0500'
categories:
- tools
- Cloud
tags:
- Eucalyptus
- HP
comments: []
---
### Problem
This is going to be about how Professor Jeremy and a couple of us managed to build a private cloud using Eucalyptus 4.0.1.
So lets look at thearchitecture of Eucalyptus. It contains the following
[caption id="attachment_1199" align="aligncenter" width="474" class=" "]<a href="https://blog.sat.iit.edu/wp-content/uploads/2014/09/diagram-eucalyptus-components.png"><img class=" wp-image-1199" src="https://blog.sat.iit.edu/wp-content/uploads/2014/09/diagram-eucalyptus-components-300x200.png" alt="Eucalyptus Architecture" width="474" height="316" /></a> Eucalyptus Architecture[/caption]

1. Cloud Controller - This is the main component and can be viewed as an entry point for the end user into the cloud
1. Walrus - This is similar to s3 storage in Amazon and is exactly that. It is also referred to as scalable object storage.
1. Storage Controller - This component is responsible for directing a storage request to the appropriate location in Walrus and the same holds for retrieval as well.
4.Node - This is a single machine with immense processing capability.
1. Node controller - Node controller which is a service running on each node. It is used to help monitor the node and deploy tasks on the node as they are received.
1. Cluster Controller - A group of node controllers are put under the supervision of a Cluster controller. The cluster controller periodically gathers the status of all the nodes also known as heartbeats. This is similar to the term availability zone in Amazon or a data center.

### Installation steps

lexington.sat.iit.edu was on Eucalyptus 4.0.1. The instructions to installing it are well documented in the below link.
https://www.eucalyptus.com/docs/eucalyptus/4.0.1/#install-guide/eucalyptus.html
We setup the CC and NC in MANAGED-NOVLAN mode.

### Challenges Faced:
Documentation for solving the issues you face while installing and operating Eucalyptus very sparse. Below are the one's we faced.

1. Setting up the ntp - All the node controllers will eventually have to be in a private network so they cannot communicate with the ntp servers due to which their clock would be incorrect. The way to get around this would open the/etc/ntp.conf using your favorite editor and add "server <ip address of CC> iburst" . Then do a "service ntp restart". It would take a couple of seconds after which the command "ntpstat' should show that your date and time is synced.
2. Use the logs well - A definite way to confirm that all the components are up and running would be from the logs which are at /var/log/eucalyptus/ and euca-describe-services being the easy way. Below is a list of log files which you could look at in each component
    + CLC - cloud-output.log : This is all that is needed for debugging any issue with the CLC. Try doing "grep -i "ERROR" cloud-output.log" . This would give you all the **"possible"** placesof errors. If you are looking for a problem with the CLC. It should be here. However, the CLC retries multiple times to fix a problem, so an error if once seen might be fixed by the CLC in the subsequent tries. 
    + Walrus -cloud-output.log : Hardly encountered any issues. But this log file would convey all the information you need.  
    + CC - cc.log : This is "THE MOST IMPORTANT FILE". Any errors with your network setup , issues with you nodes etc. can all be found here. Increase the logging level in /etc/eucalyptus.euca*conf to DEBUG ( INFO is the default) to see more information populated.

3. Firewall - The components down the hierarchy communicate with the ones above. This is possible only of the ACL allow them to do so. If you an error where a service is not reachable. Try to turn off the firewall and re-test (this should only be a final resort).
4. Uploading images and making them visible - Initially any images uploaded will only be visible to the admin of the eucalyptus account. This needs to be changed. It can be done via the below command:

    + ```euca-modify-image-attribute -l -all```

5. euca2ools - They come be default and can be accessed from the CLC after sourcing the credentials. DONOT install any additional euca2ools (different version) especially the one with the python installer. This would be result in a lot of mess as there is no "un-installer" and this would be mean we need to reset the CLC.
