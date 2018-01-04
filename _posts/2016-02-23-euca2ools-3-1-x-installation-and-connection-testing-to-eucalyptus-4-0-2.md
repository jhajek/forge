---
layout: post
status: publish
published: true
title: Euca2ools 3.1.x installation and connection testing to Eucalyptus 4.0.2
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1639
wordpress_url: https://forge.sat.iit.edu/?p=1639
date: '2016-02-23 14:06:10 -0600'
date_gmt: '2016-02-23 20:06:10 -0600'
categories:
- Cloud
- VPN
- Eucalyptus
tags:
- cloud
- IIT
- Eucalyptus
- VPN
- euca2ools
- Illinois Tech
- HP Eucalyptus 4.0.2
comments: []
---
For installing Euca2ools 3.1.x for use on Eucalyptus 4.0.2 (yes we are a couple of versions behind) here is the instructions and tests:
Pre-reqs may be needed: (Ubuntu)

```
sudo apt-get install git build-essential python-setuptools python-dev libxslt1-dev libxml2 libxml2-dev zlib1g-dev
```

On Mac or Linux [preferably Ubuntu because the school's VPN software doesn't compile on RedHat/Fedora](https://forge.sat.iit.edu/2015/10/getting-shrewsoft-vpn-2-2-1-to-compile-on-ubuntu-15-04-for-use-at-illinois-tech/)

```git clone https://github.com/jhajek/euca2ools.git```

```git branch -a```

The output of the branch command will look like below: [Yours may very as this list changes over time.  The command itself is just a command that lists previous branches and is a check point to make sure you are in the correct place.]

<blockquote>  remotes/origin/3.0-maint
  remotes/origin/3.1-maint
  remotes/origin/HEAD -> origin/mast
  remotes/origin/maint-3.0
  remotes/origin/maint-3.1
  remotes/origin/maint-3.2
  remotes/origin/master
</blockquote>

This command will move the github repo HEAD to point to euca2ools 3.1 which is the
version compatible with eucalyptus 4.0.2

```git checkout origin/maint-3.1``` 

Type this command inside the euca2ools directory to install
```
python setup.py install # add sudo if you are on Linux - no sudo on Mac
```

Note - depending on what version of Ubuntu you are using (I am looking at you Linux Mint 17.x) May have an older version of the package python-requests (which includes python-requestbuilder)  
![*Requestbuilder error*](/assets/2016/02/python-requestbuilder-error.png)

If you receive the error message above - you need to remove the python-requests default package and allow it to be installed by the python installer:
```sudo apt-get remove python-requests```

This may or may not be necessary depending on how credentials were generated for you...

Finally you need to source the credentials file you were sent (unzip the
file into a secure directory by typing: (your credential file may not be named *you-credentials.zip*

```
unzip -d creds your-credentials.zip
source eucarc
```

You can add this to your ```~/.bashrc``` as well so that when you login these credentials will be automatically sourced. Here is how to do that - note that it includes *my HOME directory* and change this to your HOME directory - best to use absolute PATH

```
# User specific aliases and functions
source /home/eucauser/creds/eucarc
```

Final test - from the command line type:

```
euca-version
euca-describe-instances
euca-describe-availability-zones verbose
```

Complete documentation on euca2ools usage is here:
[https://region-b.geo-1.objects.hpcloudsvc.com/v1/10625742765718/generated-pdfs/Eucalyptus_4.0/user-guide-4.0.2.pdf](https://region-b.geo-1.objects.hpcloudsvc.com/v1/10625742765718/generated-pdfs/Eucalyptus_4.0/user-guide-4.0.2.pdf)

You can log directly into the web console via going to
<strike><a href="https://lexington.sat.iit.edu">https://lexington.sat.iit.edu</a></strike>

take care

Jeremy Hajek
