---
layout: post
status: publish
published: true
title: Getting ShrewSoft VPN 2.2.1 to compile on Ubuntu 15.04/10/16.04 for
  use at Illinois Tech
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1577
wordpress_url: https://forge.sat.iit.edu/?p=1577
date: '2015-10-14 08:54:24 -0500'
date_gmt: '2015-10-14 13:54:24 -0500'
categories:
- Cloud
- VPN
tags:
- cloud
- Eucalyptus
- VPN
comments: []
---
<p><code>If you are like me and want to use Ubuntu Linux and a VPN in conjunction with your university network - You have to use Shrewsoft VPN client.  To use the Eucalyptus cloud and Euca2ools in the School of Applied Technology we need to access port 8773 which is not open from the outside.  Here is how to get VPN software working with IITs VPN. <strong>Update:</strong> Now tested and works on Ubuntu 16.04 beta too</p>
<p>For Linux you have to compile the application from source.   Unfortunately there are no good instructions until now...</p>
<p>1. Download and untar the tarball from shrewsoft -> latest here (10/15/15): <a href="https://www.shrew.net/download/ike/ike-2.2.1-release.tgz">https://www.shrew.net/download/ike/ike-2.2.1-release.tgz</a></p>
<p>2. Install these dependencies: <code>sudo apt-get install libedit2 libedit-dev qt-sdk cmake flex bison build-essential libssl-dev</code></p>
<p>3. In the directory where you extracted the tarball run the cmake command to build the application<br />
<code>cmake -DCMAKE_INSTALL_PREFIX=/usr -DQTGUI=YES -DETCDIR=/etc -DNATT=YES<br />
make<br />
sudo make install<br />
</code></p>
<p>4. <strong>Important</strong>  Next go to /etc and copy the <strong>iked.conf.sample</strong> file renaming it to <strong>iked.conf</strong><br />
Otherwise the key daemon won't start - you will get an error message saying key daemon not started.</p>
<p>5. <code>sudo iked </code>(to start the key daemon)</p>
<p>6. type <strong>ikea</strong> on the command line (for GUI interface)   (on Ubuntu 15.10 I typed qikea)</p>
<p>7. Once the GUI tools starts click FILE > IMPORT (here you import your Cisco PCF and it will auto-populate everything)</p>
<p>8. Then you are ready to connect - use your iit account credentials and you are in!</p>
<p>Take care</p>
