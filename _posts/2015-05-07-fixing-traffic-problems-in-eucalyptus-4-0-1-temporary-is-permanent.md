---
layout: post
status: publish
published: true
title: Fixing Traffic Problems in Eucalyptus 4.0.1 - Temporary is Permanent
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
date: '2015-05-07 10:15:16 -0500'
date_gmt: '2015-05-07 15:15:16 -0500'
tags:
- Eucalyptus
- HP
comments: []
---
<p>Our <a href="http://eucalyptus.com" title="Eucalyptus 4.0..1">Eucalyptus Cloud</a> in production (version 4.0.1) was successfully built.  It was built but for many many months we had no applications to run on it so we didn't know about scaling and I/O throughput.  We had plenty of memory and ram but the problem was we had a single Dell Powerconnect 2724 switch that was VLAN'd into two, one for the public traffic, one for the private node controller/CC network.   </p>
<p>When running multi-node Map-Reduce jobs this is not a good idea as the mappers and reducers have to talk back to each other we flooded the switch which was also processing public traffic.  So to fix this - we simply broke the private traffic off the VLAN and onto its own switch and let me tell you...   massive improvement. *Logically</p>
<p>This reminds me of a quote I heard from <a href="http://stevenmurawski.com/">Steven Murawkski...</a> </p>
<blockquote><p>"Temporary is permanent."  </blockquote></p>
<p>Which is exactly what happened here.   </p>
<p>Lesson learned.<br />
<a href="https://forge.sat.iit.edu/assets/2015/05/WP_20150506_14_54_23_Pro.jpg"><img src="https://forge.sat.iit.edu/assets/2015/05/WP_20150506_14_54_23_Pro-150x150.jpg" alt="WP_20150506_14_54_23_Pro" width="150" height="150" class="alignnone size-thumbnail wp-image-1357" /></a></p>
<p><a href="https://forge.sat.iit.edu/assets/2015/05/WP_20150506_14_54_30_Pro.jpg"><img src="https://forge.sat.iit.edu/assets/2015/05/WP_20150506_14_54_30_Pro-150x150.jpg" alt="WP_20150506_14_54_30_Pro" width="150" height="150" class="alignnone size-thumbnail wp-image-1358" /></a></p>
