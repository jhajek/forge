---
layout: post
status: publish
published: true
title: Eucalyptus sudo euca_conf --initialize error?   Fixed! You can put it on the
  boaaaaarrrrdddd! Yes!  Part 2 of 2
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 969
wordpress_url: https://blog.sat.iit.edu/?p=969
date: '2013-09-05 20:46:22 -0500'
date_gmt: '2013-09-06 01:46:22 -0500'
categories:
- Projects
- Cloud
tags:
- Eucalyptus
comments: []
---
<p><a href="https://blog.sat.iit.edu/2013/09/eucalyptus-sudo-euca_conf-initialize-error-fixed-you-can-put-it-on-the-boaaaaarrrrdddd-yes-part-1-of-2/" title="Eucalyptus sudo euca_conf &Gamma;&Ccedil;&ocirc;initialize error?   Fixed! You can put it on the boaaaaarrrrdddd! Yes!  Part 1 of 2">Part 1</a></p>
<p>Continuing from the previous article - once the 'bc' error is fixed - when you run <code>sudo /usr/sbin/euca_conf --initialize --debug</code>  now here is your error:</p>
<p><a href="https://blog.sat.iit.edu/wp-content/uploads/2013/09/afterBC.png"><img src="https://blog.sat.iit.edu/wp-content/uploads/2013/09/afterBC-300x212.png" alt="afterBC" width="300" height="212" class="alignnone size-medium wp-image-970" /></a></p>
<p>Notice it says: </p>
<blockquote><p>Status=1<br />
Output:<br />
[error:0631] Cannot find Eucalyptus bootstrapper: com/eucalyptus/bootstrap/SystemBootstrapper.<br />
....<br />
[error:0557] Service exit with a return of 1.<br />
initialize command failed</blockquote></p>
<p>So what can this be?  First thing would be to check the <code>/var/log/eucalyptus/cloud-output.log</code>, but since initializing failed that file doesn't even get created...  hmmm....</p>
<p>Ok, how about a <code>sudo find / -name SystemBootstrapper.java</code>?  Ok great we get a result: <code>/home/ubuntu/eucalyptus/clc/modules/msgs/src/main/java/com/eucalyptus/bootstrap/SystemBootstrapper.java</code> So it is on the system as a java class (Note it is not this - <a href="https://pypi.python.org/pypi/bootstrapper/0.1" title="https://pypi.python.org/pypi/bootstrapper/0.1">https://pypi.python.org/pypi/bootstrapper/0.1</a> )  Why can't the initialize script find it?  </p>
<p>Well after a bit of bravery on my part and the gracious help of the Eucalyptus <a href="http://en.wikipedia.org/wiki/IRC">IRC</a> channel the answer was figured out.  See transcript below -- <a href="https://blog.sat.iit.edu/wp-content/uploads/2013/09/eucalyptus.2013-09-05.txt">eucalyptus.2013-09-05.log</a>  Here are the highlights:</p>
<blockquote><p>2013-09-05T03:20:44  <jeevan_ullas> you might want to set the jdk in CLOUD_OPTS in eucalyptus.conf in /etc/eucalyptus/<br />
2013-09-05T03:20:50  <jeevan_ullas> to point to jdk 1.7</blockquote></p>
<p>The issue is this:   since eucalyptus requires ANT 1.8.3 (on Ubuntu 12.04.3) ANT has a Java 6 dependency.  BUT Eucalyptus requires Java 7 for compilation.  What happened is that the Java 6 became the default java.  <code>java --version</code> rendered this:<br />
<code>java version "1.6.0_27"<br />
OpenJDK Runtime Environment (IcedTea6 1.12.6) (6b27-1.12.6-1ubuntu0.12.04.2)<br />
OpenJDK 64-Bit Server VM (build 20.0-b12, mixed mode)</code></p>
<p>And there in lies the problem - when you edit the <code>/etc/eucalyptus/eucalyptus.conf</code> file you find an option at the top that says:<code> #CLOUD_OPTS="--java-home=/usr/lib/jvm/default-java --db-home=/usr/lib/postgresql/9.1"</code>  And there is the problem.  The system thinks that Java 6 is the default-java so the initialize script is trying to run the <code>com/eucalyptus/bootstrap/SystemBootstrapper</code> class but it needs Java 7 to compile properly.  </p>
<p>The answer Mr. Peabody? <a href="https://blog.sat.iit.edu/wp-content/uploads/2013/09/mrpeabody.jpg"><img src="https://blog.sat.iit.edu/wp-content/uploads/2013/09/mrpeabody-150x150.jpg" alt="mrpeabody" width="150" height="150" class="alignnone size-thumbnail wp-image-975" /></a></p>
<p>Change the value of the <code>CLOUD_OPTS</code> to be the default value of where Java 7 is installed.  That would be <code>/usr/lib/jvm/java-7-openjdk-amd64</code> if you are following <a href="https://alinush.org/2013/07/21/how-to-compile-and-install-eucalyptus-3-3-0-on-ubuntu-13-04-from-github-sources-cloud-in-a-box/" title="https://alinush.org/2013/07/21/how-to-compile-and-install-eucalyptus-3-3-0-on-ubuntu-13-04-from-github-sources-cloud-in-a-box/">Alin Tomescu's blog</a>.</p>
<p>And there you go.  And now you are ready...  Thanks goes to jeevan_ullas at <a href="http://webchat.freenode.net/?channels=eucalyptus" title="http://webchat.freenode.net/?channels=eucalyptus">http://webchat.freenode.net/?channels=eucalyptus</a> and of course <a href="https://alinush.org/" title="https://alinush.org/">Alin Tomescu's blog</a><br />
<a href="https://blog.sat.iit.edu/wp-content/uploads/2013/09/20585-kamehameha.jpg"><img src="https://blog.sat.iit.edu/wp-content/uploads/2013/09/20585-kamehameha-300x238.jpg" alt="20585-kamehameha" class="alignnone size-medium wp-image-976" /></a></p>
