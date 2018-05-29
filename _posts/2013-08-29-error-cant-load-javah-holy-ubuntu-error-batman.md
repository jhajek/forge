---
layout: post
status: publish
published: true
title: 'Error: can''t load javah? Holy Ubuntu error Batman!'
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 941
wordpress_url: https://blog.sat.iit.edu/?p=941
date: '2013-08-29 23:51:37 -0500'
date_gmt: '2013-08-30 04:51:37 -0500'
categories:
- tools
- Projects
- Cloud
tags:
- Eucalyptus
- HP
- Ubuntu
comments: []
---
<p>Hello intrepid compilers.   For those compiling Eucalyptus 3.3.0 from scratch following these wonderful directions: <a href="https://alinush.org/2013/07/21/how-to-compile-and-install-eucalyptus-3-3-0-on-ubuntu-13-04-from-github-sources-cloud-in-a-box/" title="how-to-compile-and-install-eucalyptus-3-3-0-on-ubuntu-13-04-from-github-sources-cloud-in-a-box/">https://alinush.org/2013/07/21/how-to-compile-and-install-eucalyptus-3-3-0-on-ubuntu-13-04-from-github-sources-cloud-in-a-box</a> </p>
<p>These links mention the fact that there are two patches needed to fix axis library errors that are not in the git hub install docs <a href="https://github.com/eucalyptus/eucalyptus/blob/master/INSTALL" title="INSTALL.md">https://github.com/eucalyptus/eucalyptus/blob/master/INSTALL</a> </p>
<p>Any way the document runs into a problem - configure works just fine - but when you go to run<br />
<code>sudo make install</code> </p>
<p>it errors out with:</p>
<p><code>/home/ubuntu/eucalyptus/clc/modules/storage-controller/build.xml:77: Can't load javah</code>  </p>
<p><strong>What might be the cause?</strong>  Scroll up and find the hint in the output <code>Unable to locate tools.jar. Expected to find it in /usr/lib/jvm/java-6-openjdk-amd64/lib/tools.jar</code>  </p>
<p><strong>What does this mean?</strong>   A little bit of <em>"stackoverflowing"</em> (Perhaps I can coin a new internet meme to replace <em>"googling"</em>) gives this hint <a href="http://stackoverflow.com/questions/15032230/cant-load-javah-error-in-eclipse" title="http://stackoverflow.com/questions/15032230/cant-load-javah-error-in-eclipse">http://stackoverflow.com/questions/15032230/cant-load-javah-error-in-eclipse</a></p>
<p>Turns out I needed to add openjdk-6-jdk to the install dependencies to satisfy this ANT (1.8.3) dependency on Ubuntu 12.04.3.   I hope that helps...  Once the extra dependency is added then you will see the screen below. (Sorry Louis - the Lions just happened to be in the background...)</p>
<p><a href="assets/2013/08/chi_lions_bears_02.jpg"><img src="assets/2013/08/chi_lions_bears_02-300x187.jpg" alt="chi_lions_bears_02" width="300" height="187" class="alignnone size-medium wp-image-942" /></a></p>
