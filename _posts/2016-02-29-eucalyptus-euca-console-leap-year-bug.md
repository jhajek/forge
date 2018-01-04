---
layout: post
status: publish
published: true
title: Eucalyptus Euca-console leap-year bug and fix
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1648
wordpress_url: https://forge.sat.iit.edu/?p=1648
date: '2016-02-29 11:45:36 -0600'
date_gmt: '2016-02-29 17:45:36 -0600'
categories:
- Uncategorized
- tools
- Eucalyptus
tags:
- Eucalyptus
- euca-console
- HP Eucalyptus
- HP Eucalyptus 4.0.2
comments: []
---
Woke up this morning to an **Internal Server Error** after logging into the Euca-Console.

Looking at the error logs this came up:
![*Error Message*](/assets/2016/02/desktop-1_002-768x432.png)

Leap year bug...  luckily everyone else's software caught fire so there was a quick solution here:

[https://groups.google.com/a/eucalyptus.com/forum/#!topic/euca-users/mqhYXCzAfaE](https://groups.google.com/a/eucalyptus.com/forum/#!topic/euca-users/mqhYXCzAfaE)

Fix is here: by changing the line: 

``` expires = datetime.today().replace(year=2003)" to expires = datetime.today().replace(year=2016) in /usr/lib/python2.6/site-packages/Beaker-1.5.4-py2.6.egg/beaker/session.py ```

and restarted the console service and it works fine.
