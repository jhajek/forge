---
layout: post
status: publish
published: true
title: Programming Achievement Unlocked - or what I do all day...
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1406
wordpress_url: https://forge.sat.iit.edu/?p=1406
date: '2015-06-11 15:04:25 -0500'
date_gmt: '2015-06-11 20:04:25 -0500'
categories:
- Cloud
tags:
- Eucalyptus
- HP
comments: []
---
There comes a time in people's lives when they move up.  For instance when you move from your parents house into your own house.  Or from borrowing you dad's car to buying your own.  In the past I have compiled little 1000 line c++ projects (small games)  and written some mean Bash scripts.  Until today I can now say I have reached the level shown here...

[compiling.jpg](http://imgs.xkcd.com/comics/compiling.png "Compiling XKCD")

The reason I can say this working on our Hadoop Cluster build scripts - each system takes about 29 minutes to build end to end.  Some of the vagrant test boxes take up to 50 minutes to build mostly do to compressing multi-gigabytes. What a wonderful world it is.

```
real 28m27.374s
user 14m28.827s
sys 1m57.265s
```

[Processor: Intel(R) Core(TM) i3 CPU M 380 @ 2.53GHz](http://ark.intel.com/products/50178/Intel-Core-i3-380M-Processor-3M-Cache-2_53-GHz "Core i3 380M")
1st Gen Core I series  8 GB RAM 5400 RPM HDD
