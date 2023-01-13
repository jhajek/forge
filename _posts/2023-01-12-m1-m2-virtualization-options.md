---
layout: post
status: publish
published: true
title: 'Current State of M1/M2 Mac Virtualization'
tags: 
- virtualization
- virtualbox
- m1
- m2
- macOS
comments: []
---

This is a current survey of macOS for M1/M2 ARM platform (Apple Silicon) virtualization replacement options. Since this is not x86 Virtual Box is out of the questions. **Note:** when I type *Apple Silicon*, I mean the M1/M2 plaform.

## Two Paid Solutions

I am not against spending money to get a good product and there are two really great paid solutions for Apple Silicon.

[Parallels](https://www.parallels.com/ "website for parallels virtualization solution") has a yearly subscription cost and is a great piece of software.  There is also a [pro-edition](https://www.parallels.com/products/desktop/pro "website for pro-edition of parallels") with that gives you access to a commandline and allows for scriptable automation to take place. This is needed for integration with [Vagrant](https://vagrantup.com "website for Vagrant"), [Packer](https://packer.io "website for packer"), [Docker](https://docker.io "website for Docker.io"), and [MiniKube](https://minikube.sigs.k8s.io/docs/ "website for minikube").

There is also [VMware Fusion - from VMware](https://www.vmware.com/products/fusion.html "Website VMware fusion for M1 Mac")

## Two Native Apple Silicon

There are two native solutions as well for the Apple Silicon.

Using [UTM](https://mac.getutm.app/ "website for apple UTM"), the direct Apple virtualization framework native to Apple Silicon. This is stable enough to use now but hasn't opened their API yet to third party automation tools such as Vagrant or Packer, but the request tickets are [in the works](https://github.com/hashicorp/vagrant/issues/12518 "website for UTM requests").

There is also QEMU, which is an emulator that runs on Apple Silicon. UTM runs on top of QEMU but QEMU can run by itself using the `libvirt` library -- which is heavily Linux based but has been ported to Apple Silicon.

There is a third option, and new tool called [Tart](https://github.com/cirruslabs/tart "website for Tart"). This is brand new and Vagrant Integration [is being actively worked on](https://github.com/hashicorp/vagrant/issues/12760 "website for tart").

## Solution

There are a few options here. I know that the Parallels Pro edition works excellently with Packer and Vagrant and the plugins. I used it almost immediately after the M1 came out and even helped a student place an issue in the Vagrant Plugin.

The other two solution UTM and QEMU look promising, but alas I don't have any Apple Silicon.
