---
layout: post
status: publish
published: true
title: Eucalyptus 4.0.2 How to solve EUCA-7376 error -- High Concurrent Locks on DB
  Causing Cloud Performance Issues
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1410
wordpress_url: https://forge.sat.iit.edu/?p=1410
date: '2015-06-11 15:23:27 -0500'
date_gmt: '2015-06-11 20:23:27 -0500'
categories:
- Cloud
tags:
- Eucalyptus
comments: []
---
<p><a href="https://forge.sat.iit.edu/assets/2015/06/eucalyptus-logo.jpg"><img src="https://forge.sat.iit.edu/assets/2015/06/eucalyptus-logo-150x150.jpg" alt="eucalyptus-logo" width="150" height="150" class="alignnone size-thumbnail wp-image-1413" /></a></p>
<p>Every six months our Eucalyptus Cloud has this problem: <a href="https://eucalyptus.atlassian.net/browse/EUCA-7376">https://eucalyptus.atlassian.net/browse/EUCA-7376</a>   We were on 4.0.1 and now 4.0.2.  I had posted this issue to the Eucalyptus Users Google Groups Back in October of 2014 <a href="https://groups.google.com/a/eucalyptus.com/forum/#!searchin/euca-users/EUCA-7376/euca-users/nHM6BXPRoOo/yXnlwDEh1SwJ">https://groups.google.com/a/eucalyptus.com/forum/#!searchin/euca-users/EUCA-7376/euca-users/nHM6BXPRoOo/yXnlwDEh1SwJ</a></p>
<p>The first time we solved the issue by just reformatting everything and reinstall.  (That worked but we didn't know why...)</p>
<p>The first symptom of this is that the Eucaconsole (installed locally on the CLC) would not receive login requests -- nothing would happen upon login.  This was our first hint that it was a service problem.</p>
<p>Then we started seeing these errors in the Postgress logs:</p>
<p><code><br />
ERROR:  update or delete on table "metadata_extant_network" violates foreign key constraint "fkbdc151865dab4005" on table "metadata_network_indices"<br />
DETAIL:  Key (id)=(8a1f98a840b2dcf20140be92918a0f8e) is still referenced from table "metadata_network_indices".<br />
STATEMENT:  delete from metadata_extant_network where id=$1 and version=$2<br />
ERROR:  update or delete on table "metadata_extant_network" violates foreign key constraint "fkbdc151865dab4005" on table "metadata_network_indices"<br />
DETAIL:  Key (id)=(8a1f98a840b2dcf20140be5722140f59) is still referenced from table "metadata_network_indices".<br />
STATEMENT:  delete from metadata_extant_network where id=$1 and version=$2<br />
ERROR:  update or delete on table "metadata_extant_network" violates foreign key constraint "fkbdc151865dab4005" on table "metadata_network_indices"<br />
DETAIL:  Key (id)=(8a1f98a840b2dcf20140bde9407c0ed6) is still referenced from table "metadata_network_indices".<br />
STATEMENT:  delete from metadata_extant_network where id=$1 and version=$2<br />
ERROR:  update or delete on table "metadata_extant_network" violates foreign key constraint "fkbdc151865dab4005" on table "metadata_network_indices"<br />
DETAIL:  Key (id)=(8a1f98a840b2dcf20140bd7b64b60e5d) is still referenced from table "metadata_network_indices".<br />
STATEMENT:  delete from metadata_extant_network where id=$1 and version=$2<br />
ERROR:  update or delete on table "metadata_extant_network" violates foreign key constraint "fkbdc151865dab4005" on table "metadata_network_indices"<br />
DETAIL:  Key (id)=(8a1f98a840b2dcf20140bf6e4c1a107c) is still referenced from table "metadata_network_indices".<br />
STATEMENT:  delete from metadata_extant_network where id=$1 and version=$2<br />
</code></p>
<p>So we made like <a href="https://en.wikipedia.org/wiki/Dig_Dug">DigDug</a> found out what may have been causing this.</p>
<p><strong>Solution:</strong><br />
We noticed that the dhcp service failed to start on the CC.  Not dhpcd but dnsmasq and these 3 steps fixed our problem.<br />
1. The postgres is caused by DHCP. That needs to be fixed. We need to restart dhcp and then the restart the CC to fix the dhcp.<br />
2. Restart the CLC to get rid of the metadata.<br />
3. nginx service on CLC service needs to be manually started after the dhcp is fixed and the CLC is rebooted</p>
<p>* In addition the SC went to state broken during this time - I believe because of the postgress errors and process breaking communication</p>
<p>Logs available upon request.</p>
