---
layout: post
status: publish
published: true
title: Making a GPU Cluster. (Part 4)
author:
  display_name: Eric Macorkel
  login: emacorke
  email: emacorke@hawk.iit.edu
  url: ''
author_login: emacorke
author_email: emacorke@hawk.iit.edu
date: '2014-11-08 21:37:29 -0600'
date_gmt: '2014-11-09 03:37:29 -0600'
tags:
- gpu
- Eucalyptus
comments: []
---
## Making a GPU Cluster. (Part 4)

With the virtual machine up and running the next thing we wanted to do was see if we could pass the GPU to our virtual machine. After researching online we came upon a couple of guides that dealt with PCI passthrough. The guide we ended up using was this...

[RedHat PCI Passthrough Document](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Virtualization/chap-Virtualization-PCI_passthrough.html "RedHat PCI Passthrough Document")

After altering our `grub.conf` file and adding our GPU card to the virtual machine we tried to start up our virtual machine but couldn't. Worst yet, when we restarted the server it would not boot to the command line like usual for us but instead we got a DMAR error. Specifically, this error.

```DMAR:[DMA READ] Request device[00:02.0] fault addr 1fc00000```

We researched more about this error and through that we found that even though our motherboard chipset, Z77, should of supported `vt-d` certain motherboard manufacturers don't implement it well. To complicate matters further it seems that different BIOS versions can enable or break `vt-d` support. We found this website that gave a useful list of motherboards that support `vt-d`...

[http://www.overclock.net/t/1338063/vt-d-compatible-motherboards](www.overclock.net/t/1338063/vt-d-compatible-motherboards "VT-d Compatible Motherboards")

We ended up swapping out our motherboard for a ASRock Z77 Extreme 4 board. Our current hardware setup is...

* ASRock Z77 Extreme 4
* AMD HD 6450
* i5-3330

We enabled `vt-d` support on our motherboard by going into BIOS and we expected that to solve our DMAR problem but it didn't. Further research showed that hardware support is critical to pass a GPU to a virtual machine, yes, but passing a GPU to a virtual machine is a lot more complicated than a typical PCI pass through of say a network card. GPU passthrough support seems to have been very spotty and hard to do but it seems people have found success passing a GPU through using VFIO. Currently were are using CentOS 6 and we don't think that CentOS 6 will work for us if we want to use VFIO. The reason for this is because VFIO requires the latest and most most cutting edge versions of KVM and Qemu. CentOS seems to use a thing called Qemu-KVM but patching that to the latest version at the time of this writing doesn't give us what we need to support VFIO. Due to this we are currently looking into switching our current Linux OS to another Linux OS. So far Ubuntu, Fedora, and Arch Linux seem like interesting candidates.
