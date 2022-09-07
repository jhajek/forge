---
layout: post
status: publish
published: true
title: Spaces Will Always Kill You...
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
date: '2011-07-14 21:01:59 -0500'
date_gmt: '2011-07-15 03:01:59 -0500'
tags: []
comments: []
---
Spaces will always kill you... you think, "Oh this only happens to other people, not me, I can get away with putting spaces in a filename..." Perhaps you ignored that advice and did it anyway? And then what happened?

Exactly.

This point is no more vitally important than in dealing with vmware esx and esxi servers. Especially when dealing with datastores. See this link for the gory details <a href="http://kb.vmware.com/kb/1010832" target="_blank">http://kb.vmware.com/kb/1010832</a>

I have my vcenter instance and I am managing 3 esxi-servers.
If I want to download the entire .vmdk (virtual machine) for backup I would simply change my options in vCenter and goto DATASTORES.

<img title="pic4" src="/assets/2011/07/pic4-150x150.png" alt="" width="150" height="150" />

When I browse a datastore looking for a virtual machine here is what I see:

![Looking for VM](/assets/2011/07/pic21-150x150.png "Looking for VM")

I select datastore one and I see two virtual machines. Cicero and Danada. Let me click on Cicero and I can browse the datastore.

<img src="/assets/2011/07/pic5-150x150.png" alt="" width="150" height="150" />

Ok now I want to click on Danada and browse its Datastore

<img src="/assets/2011/07/picture3-150x150.png" alt="" width="150" height="150" />

Notice all the content is missing! Trust me it is still there as this is an important server and if it went missing, I just might go missing as well. So what is going on? Remember the link I posted at the top? Count the number of characters in the virtual machine name of danada. Notice how in the datastore window it seems to cut off.

(Compare that with the pic 1 and you will see a longer name.) (Hint Also I fixed the problem as well replacing " " with "_".)

It turns out if you have a space in the 32nd place of the name of your vm, the datastore cannot see it! So lets do some quick counting

<img src="/assets/2011/07/32space1.png" alt="" width="327" height="70" />

Sure enough 32nd char is a space. Now what to do? Simple Vmware recommends to rename the vm and eliminate the space in that spot (I now go further and forbid all spaces ever) and then you need to
migrate the vm to another system and the content will appear again. I tried this and it works the problem of the missing data solved.
