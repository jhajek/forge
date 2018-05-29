---
layout: post
status: publish
published: true
title: Eucalyptus sudo euca_conf --initialize error?   Fixed! You can put it on the
  boaaaaarrrrdddd! Yes!  Part 1 of 2
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 960
wordpress_url: https://blog.sat.iit.edu/?p=960
date: '2013-09-05 20:46:01 -0500'
date_gmt: '2013-09-06 01:46:01 -0500'
categories:
- tools
- Projects
- Cloud
tags:
- Eucalyptus
- HP
comments: []
---
<p>If the title doesn't make sense (if you are not from Chicago it might not)  You can see where it came from at this link -<a href="https://www.youtube.com/watch?v=6Aj-EQrSAUs" title="Hawk says, Yes!">https://www.youtube.com/watch?v=6Aj-EQrSAUs</a></p>
<p>Ok so you are out to compile <a href="http://www.eucalyptus.com">Eucalyptus</a> on Ubuntu and are following the instructions at <a href="https://github.com/eucalyptus/eucalyptus/blob/master/INSTALL">https://github.com/eucalyptus/eucalyptus/blob/master/INSTALL</a>  and also the gracious instructions at <a href="https://alinush.org/2013/07/21/how-to-compile-and-install-eucalyptus-3-3-0-on-ubuntu-13-04-from-github-sources-cloud-in-a-box/" title="The blog of Alin Tomescu">The blog of Alin Tomescu</a>  and so far with a bit of grace luck and the previous article, <a href="https://blog.sat.iit.edu/2013/08/error-cant-load-javah-holy-ubuntu-error-batman/" title="Error: can't load javah? Holy Ubuntu error Batman!">https://blog.sat.iit.edu/2013/08/error-cant-load-javah-holy-ubuntu-error-batman/</a>.</p>
<p>You have successfully solved the dependencies and now have completed <code>make</code> and <code>sudo make install</code> with nary an error.  The next step is now to initialize the CLC with the command <code>sudo /usr/sbin/euca_conf --initialize --debug</code>  (always add the --debug it gives verbose output that is immensely helpful.)</p>
<p>This is what is your output: </p>
<p><a href="https://blog.sat.iit.edu/wp-content/uploads/2013/09/sudo-euca_conf-initialize-error.png"><img src="https://blog.sat.iit.edu/wp-content/uploads/2013/09/sudo-euca_conf-initialize-error-300x212.png" alt="sudo-euca_conf--initialize-error" width="300" height="212" class="alignnone size-medium wp-image-963" /></a></p>
<p>First error you will notice that it is asking for 'bc' - when following <a href="https://alinush.org/2013/07/21/how-to-compile-and-install-eucalyptus-3-3-0-on-ubuntu-13-04-from-github-sources-cloud-in-a-box/" title="The blog of Alin Tomescu">The blog of Alin Tomescu</a> you will notice in the pre-reqs package section:</p>
<blockquote><p># install eucalyptus build dependencies<br />
# WARNING: install these now, not later</p>
<p>sudo apt-get -y install cdbs debhelper libaxis2c-dev librampart-dev \<br />
	libvirt-dev libfuse-dev libfuse2 libcurl4-openssl-dev \<br />
	libssl-dev ant-optional zlib1g-dev pkg-config swig python \<br />
	python-setuptools open-iscsi libxslt1-dev gengetopt ant \<br />
	postgresql-server-dev-9.1 openjdk-7-jdk groovy libcap-dev</p>
<p># install eucalyptus runtime dependencies<br />
# WARNING: install these now, not later:

```sudo apt-get install -y bridge-utils iputils-arping libapache2-mod-axis2c adduser \
 	apache2 apache2-mpm-worker bridge-utils dhcp3-server euca2ools file \
	iptables iputils-arping libapache2-mod-axis2c libaxis2c0 libc6 \
	libcrypt-openssl-random-perl libcrypt-openssl-rsa-perl libcrypt-x509-perl \
	libcurl3 libdevmapper1.02.1 libpam-modules librampart0 libssl1.0.0 libvirt0 \
	libvirt-bin libxml2 libxslt1.1 lvm2 open-iscsi openssh-client openssh-server \
	parted postgresql-client-9.1 python python2.7 python-boto python-psutil \
	rsync sudo tgt vblade vlan vtun postgresql-9.1 apache2-threaded-dev \
	bzr drbd8-utils gcc kvm libsys-virt-perl libxml-simple-perl make openntpd \
	python-libvirt python-pygresql qemu-kvm unzip at
```

<p>The install instructions from <a href="https://alinush.org/2013/07/21/how-to-compile-and-install-eucalyptus-3-3-0-on-ubuntu-13-04-from-github-sources-cloud-in-a-box/" title="The blog of Alin Tomescu">The blog of Alin Tomescu</a>  are an all in one install (meaning all components on a single machine.)  I wanted to split my components amongst my different servers.  So I hopped over to <a href="https://github.com/eucalyptus/eucalyptus/blob/master/INSTALL" title="INSTALL">https://github.com/eucalyptus/eucalyptus/blob/master/INSTALL</a> which has the pre-reqs broken down by system:</p>
<blockquote><p>
Ubuntu 12.04<br />
Install the following build dependencies.</p>
<p>cdbs debhelper libaxis2c-dev librampart-dev \<br />
default-jdk libvirt-dev libfuse-dev libfuse2 libcurl4-openssl-dev \<br />
libssl-dev ant-optional zlib1g-dev pkg-config swig python \<br />
python-setuptools rsync wget open-iscsi libxslt1-dev gengetopt ant \<br />
groovy postgresql-server-dev-9.1 iputils-arping</p>
<p>CLC: libc6, adduser, openssh-server, openssh-client, sudo, rsync,<br />
postgresql-client-9.1, python2.7, python (>= 2.5), rsync, python-boto,<br />
python-psutil, python-pygresql, lvm2, libgetargs-long-perl, postgresql-9.1,<br />
vblade, dmsetup, default-jre-headless | java6-runtime-headless, velocity,<br />
libpostgresql-jdbc-java (>= 9.1), libjboss-common-java,<br />
libhibernate-commons-annotations-java<br />
</blockquote></p>
<p>This dependency list is missing <code>bc</code> - solution?  Just <code>sudo apt-get install bc openjdk-7-jdk</code> and problem goes away. =)</p>
<p>Now on to the main event...  <a href="https://blog.sat.iit.edu/2013/09/eucalyptus-sudo-euca_conf-initialize-error-fixed-you-can-put-it-on-the-boaaaaarrrrdddd-yes-part-2-of-2/" title="Eucalyptus sudo euca_conf &Gamma;&Ccedil;&ocirc;initialize error?   Fixed! You can put it on the boaaaaarrrrdddd! Yes!  Part 2 of 2">Part 2</a></p>
