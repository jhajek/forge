---
layout: post
status: publish
published: true
title: Using MooseFS in Cloud Computing
author:
  display_name: zwagner
  login: zwagner
  email: zwagner@iit.edu
  url: ''
author_login: zwagner
author_email: zwagner@iit.edu
wordpress_id: 6
wordpress_url: http://aurora.rice.iit.edu/?p=6
date: '2011-07-13 14:31:38 -0500'
date_gmt: '2011-07-13 20:31:38 -0500'
categories:
- Uncategorized
tags:
- MooseFS
- cloud
- IT
- IIT
- file system
comments: []
---

The cloud computing project that our team here at IIT has been working on is still chugging along. ┬My role has been to research and test a file system for our cloud's storage. The file system I've been using is <em>MooseFS</em> (<a title="www.moosefs.org" href="http://www.moosefs.org" target="_blank">www.moosefs.org</a>).

### MooseFS Background

As far as standard file operations go, <em>MooseFS</em> is similar to any other Unix-based file system. However, <em>MooseFS</em> also has:
<ul>
<li>High Reliability - multiple copies of data can be stored over different servers</li>
<li>Dynamic Expandability - more capacity is attained by adding more servers and/or disks</li>
<li>"Trash Bin" - deleted files are still retained for a configurable period of time</li>
<li>File Snapshots - even when files are being written or read</li><br />
</ul></p>
<div><em>MooseFS</em> architecture is made up of four major components: master server, chunk server, metalogger server, and a mount.</div></p>
<div>
<ul>
<li><em>Master Server</em> is a managing server. It is a single machine that manages the whole file system and stores metadata for every file.</li>
<li><em>Chunk Servers</em> are the data servers. They store file data and synchronize it among themselves (if multiple copies are to be stored).</li>
<li><em>Metalogger Server</em> is a metadata backup server. This stores metadata changelogs and periodically downloads the main metadata file. Also serves as a back-up master server.</li>
<li><em>Mounts</em> are the client computers that access the files in <em>MooseFS</em>. Mounts use the mfsmount process to communicate with the master server and chunk servers.</li><br />
</ul></p>
<div><em>MooseFS</em> is available on Linux, FreeBSD, OpenSolaris, and MacOS X.</div><br />
</div><br />
&nbsp;</p>
<div><strong>My <em>MooseFS</em> Test System for the Cloud</strong></div><br />
&nbsp;</p>
<div>My test system consists of 5 PC's: 1 Master Server, 1 Metalogger Server, 2 Chunk Servers, and 1 Client System. Eventually the Client System won't be needed because my test system will be attached to the storage controller for the cloud.</div><br />
&nbsp;</p>
<div>I installed MooseFS on top of machines running the Ubuntu server OS. The installs on each of the machines were straightforward since the <em>MooseFS</em>contained a step-by-step installation document for each machine. There was a small hiccup. After trying to configure <em>MooseFS</em>, I realized I needed to install the <em>zlib</em> data compression package (<a title="zlib.net" href="http://zlib.net" target="_blank">www.zlib.net</a>). After getting that installed, the <em>MooseFS</em> install was a breeze.</div><br />
&nbsp;</p>
<div>Trying to mount the Client to the Master Server also proved troublesome at first. It turned out to be a faulty ethernet cable. After that was fixed I had my entire test system up and running.</div><br />
&nbsp;</p>
<div>With that out of the way, my next goal is to try and configure the Metalogger Server as a back-up to the Master Server. I hope my next post proves that I was successful.</div><br />
&nbsp;</p>
<div>Take care,</div></p>
<div>Zachary Wagner</div></p>
