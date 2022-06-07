---
layout: post
status: publish
published: true
title: Making a GPU Cluster. (Part 3)
author:
  display_name: Eric Macorkel
  login: emacorke
  email: emacorke@hawk.iit.edu
  url: ''
author_login: emacorke
author_email: emacorke@hawk.iit.edu
date: '2014-08-01 22:18:34 -0500'
date_gmt: '2014-08-02 03:18:34 -0500'
tags:
- gpu
comments: []
---
The next thing we wanted to do was create a virtual machine that would run CentOS as an operating system. To create the virtual machine we did:

```# virt-install --name centos-vm1--ram=2048--os-variant=rhel6 --network bridge=br0 --graphics none --console pty,target_type = serial --location  http://mirror.centos.org/centos/6/os/x86_64/ --extra-args   console=ttyS0, 115200n8 serial  --disk path= /var/lib/libvirt/images/centos-vm1-img, size=30```

What we typed into the command line interface was instructions to create a virtual machine named centos-vm1, allocate it 2048 ram to use, have the OS type be rhel6, use a network bridge called `br0`, inform the system that we will not be using a GUI for this install, use a serial console to view the virtual machine, inform the system of where the installation files for CentOS is, and create an image at specified location to use for future booting.

When we executed the `virt-install` command we created the virtual machine and were guided through the installation of CentOS onto the virtual machine with prompts that could be read at the command line interface. One of the prompts asked us whether or not we would be using dhcp to assign an IP address to our virtual machine. We ended up using this option. After going through all the prompts CentOS was installed and we rebooted the system. To access our newly created virtual machine we did.

```virsh console centos-vm1```

When we entered this command we got to the part that showed the escape character.  Sometimes when we press enter at this part we're presented with the logon for the virtual machine but other times we have to wait a considerable amount of time before the virtual machine logon comes up. We're still looking into why this is and if we can shorten the time we wait. One of the options we explored was not using virsh console to log into the virtual machine but instead using ssh to do it.
