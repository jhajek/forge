---
layout: post
status: publish
published: true
title: Ubuntu 11.10 tg3_Stop_Block Error
author:
  display_name: aplovich
  login: aplovich
  email: aplovich@hawk.iit.edu
  url: ''
author_login: aplovich
author_email: aplovich@hawk.iit.edu
date: '2012-05-07 09:57:11 -0500'
date_gmt: '2012-05-07 14:57:11 -0500'
tags:
- HP
comments: []
---
<p>We recently encountered a problem with our Ubuntu servers were they would boot up to the login screen, and continuously display an error where tg3_stop_block timed out.┬&aacute; After a bit of searching I found that tg3 stood for tigon3, and was the nic driver.┬&aacute; This wasn't an issue in 11.04 (11.10 on the servers), so I checked the driver versions for tigon3 between Ubuntu 11.04 and 11.10, and 11.10 had a slightly newer version.┬&aacute; Also the tigon3 driver was inherently part of the kernel, and broadcom wasn't offering any sources/ debs.┬&aacute; So a rollback was out of the question.</p>
<p>I continued to research this, but didn't have much luck diagnosing the problem.┬&aacute; So eventually we just decided to see if it was an issue in the 12.04 beta.┬&aacute; Oddly enough during the first install of 12.04 the installer prompted me for a tigon3 driver, so I looked around and found one on the dabien website.┬&aacute; I have a feeling the driver I found was old/ not for Ubuntu, because when I tried feeding it in, my install corrupted and I had to start over.┬&aacute; On the second attempt I skipped the missing drivers, and it installed flawlessly.┬&aacute; The stop_block error was also gone. When I checked, the tg3 version was newer, so it seems the issue was fixed between 11.10 and 12.04.</p>
<p><strong>Update: 07/02/2012</strong><br />
Turns out the solution was kernel related...  Ubuntu 12.04 comes with a stock 3.0.23 kernel and herein lies the problem.   I explained the details on the askubuntu.com stackexchange on how to fix the problem (hint - just upgrade your kernel (apt-get dist-upgrade))<br />
<a href="http://askubuntu.com/questions/114166/broadcom-corporation-netlink-bcm57785-gigabit-ethernet-pcie-driver-tg3-will-not" title="http://askubuntu.com/questions/114166/broadcom-corporation-netlink-bcm57785-gigabit-ethernet-pcie-driver-tg3-will-not">http://askubuntu.com/questions/114166/broadcom-corporation-netlink-bcm57785-gigabit-ethernet-pcie-driver-tg3-will-not</a></p>
