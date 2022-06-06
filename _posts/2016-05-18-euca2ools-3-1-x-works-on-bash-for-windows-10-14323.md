---
layout: post
status: publish
published: true
title: Euca2ools 3.1.x works on Bash for Windows 10 14323
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
date: '2016-05-18 23:39:45 -0500'
date_gmt: '2016-05-19 04:39:45 -0500'
tags:
- euca2ools
- bash on windows
comments: []
---
Bash for Windows or the [Linux Subsystem for Windows](http://blog.dustinkirkland.com/2016/04/howto-ubuntu-on-windows.html) Since it is basically Ubuntu 14.04 - euca2ools should install properly.  I set out to see what would happen as it would be comforting to be able to use the Linux subsystem for euca2ools work.

![*Git clone*](/assets/2016/05/bash-git-clone.png)
Following the normal process -- everything worked!  But one thing...
![*Euca Version*](/assets/2016/05/bash-euca-version.png)

![*Bash on Windows Install*](/assets/2016/05/bash-on-win-install-1024x608.png)

The thing to keep in mind is that Ubuntu 14.04 has its own version of the python-requests library.  You need to remove this version:

```sudo apt-get remove python-requests```

![*Bash on euca2ools*](/assets/2016/05/bash-euca2ools-building.png)
![*Bash Euca Describe*](/assets/2016/05/bash-euca-describe-instances.png)
