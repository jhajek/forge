---
layout: post
status: publish
published: true
title: 'How to Create a Packer Promox-Iso Template with Cloud-Init'
categories:
- software
tags: 
- hadoop
- compresssion
- extensions
comments: []
---

This one gets a bit trickier as cloud-init needs to be configured and then attached, the Packer Proxmox builder assumes you already have the cloud-init iso with SSH keys already created.

For those who are not aware [cloud-init](https://cloudinit.readthedocs.io/en/latest/ "cloud init webpage") *"is the industry standard multi-distribution method for cross-platform cloud instance initialization. It is supported across all major public cloud providers, provisioning systems for private cloud infrastructure, and bare-metal installations."*

So here is the page from the Proxmox documentation on how to create a Proxmox Cloud-init disk and attach it.  

https://pve.proxmox.com/wiki/Cloud-Init_Support
