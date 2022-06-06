---
layout: post
status: publish
published: true
title: Renumbering an Ethernet Interface
author:
  display_name: Eric Macorkel
  login: emacorke
  email: emacorke@hawk.iit.edu
  url: ''
author_login: emacorke
author_email: emacorke@hawk.iit.edu
date: '2014-07-18 21:35:17 -0500'
date_gmt: '2014-07-19 02:35:17 -0500'
tags:
- gpu
comments: []
---

### Problem

Due to a mishap with a motherboard that had a bent CPU slot pins we needed to replace our motherboard. After receiving the new motherboard we took the CPU, GPUs, and the RAM of our previous motherboard and put them in the new motherboard. We booted up our server and expected everything to work the way they always have but we encountered a problem. Specifically, we couldn't access the local network and internet. We typed in ifconfig at the command prompt and noted that a different Ethernet interface was detected. Instead of showing eth0, the interface we had been using all along, eth1 was shown instead.

When we tried to bring up eth0 by typing into the command prompt ifconfig eth0 up the system responded that eth0 didn't exist. After some brainstorming we remembered that we were using the Ethernet interface of the motherboard. By using a new motherboard we were in fact using a different Ethernet interface which the system made a note of and called eth1. Since all of the networking scripts, IP table settings, and other files were configured with the Ethernet interface of our previous motherboard (eth0) we couldn't get on the internet. The solution was to rename/renumber our current Ethernet interface to say its eth0. To do this we needed to edit two files.

The first file we edited was the 70-persistent-net.rules file. So at the command prompt we typed in:

```vim /etc/udev/rules.d/70-persistent-net.rules```

When we opened up the file using vim we saw entries that looked like the following:

```SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:0c:29:de:ad:20", ATTR{type}=="1", KERNEL=="eth*", NAME="eth0"```

```SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:0c:29:de:ad:2a", ATTR{type}=="1", KERNEL=="eth*", NAME="eth1"```

The first entry showed information regarding our previous motherboard's Ethernet interface card eth0. The second entry showed information regarding our new motherboard's Ethernet interface card eth1.  What we needed to do was delete the first entry and then edit the second entry. Under the second entry there was an attribute called **"NAME"**.  We changed the entry of eth1 to eth0. Finally, we wrote down the MAC address of our card which was 00:0c:29:de:ad:2a. We would use the MAC address we wrote down to edit the second file. We saved our changes and exited vim.

The second file we needed to edit was our networking script for eth0. So at the command prompt we typed in:

```vim /etc/sysconfig/network-scripts/ifcfg-eth0```

This file has an entry called ```HWADDR``` which has the MAC address of eth0. The MAC address in this file mapped to the MAC address of the Ethernet Interface of our previous motherboard. We replaced the MAC address with the MAC address we wrote down earlier. It should be noted that If this file doesn't have an entry called ```HWADDR``` then one must create this entry and type in the MAC address written down earlier. We saved, exited VIM, and rebooted the system. Once that was done we did ifconfig at the command prompt and checked to see if our edits went through. It did and we then checked to see if we were able to connect to the network and internet. To our relief we were able to.
