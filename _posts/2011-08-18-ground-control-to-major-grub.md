---
layout: post
status: publish
published: true
title: Ground control to major GRUB
author:
  display_name: bkhodja
  login: bkhodja
  email: bkhodja@iit.edu
  url: ''
author_login: bkhodja
author_email: bkhodja@iit.edu
date: '2011-08-18 11:27:09 -0500'
date_gmt: '2011-08-18 16:27:09 -0500'
tags:
- HP
comments: []
---
Sometimes GRUB and/or the Ubuntu command line may suddenly change to a resolution which is not compatible with your monitor in which case your monitor displays a message such as "Signal out of range" or "No signal." An easy way to solve this problem is to do the following:

1. Connect a monitor which can support high resolutions and refresh rates to the problematic machine (a CRT monitor should work). If you do not have an extra monitor available, you could also SSH into the problematic machine using an SSH client on another machine.
2. Edit the file ```/etc/default/grub```
3. Uncomment the line which reads ```#GRUB_GFXMODE=1024x768``` and then change the '1024x768' to whichever resolution you would like to use. To be safe, I suggest using 800x600 or 1024x768 at the most.
4. Save the file.
5. Execute the command ```sudo update-grub``` and then reboot the machine.
Now, reconnect the monitor which previously was not working. Both GRUB and the Ubuntu command line will now, by default, run using the resolution specified in the ```GRUB_GFXMODE=``` line of the ```/etc/default/grub``` file!
T
he information used to create this post was obtained from - <a title="https://help.ubuntu.com/community/Grub2" href="https://help.ubuntu.com/community/Grub2" target="_blank">https://help.ubuntu.com/community/Grub2</a>
