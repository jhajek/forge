---
layout: post
status: publish
published: true
title: Eucalyptus 4.0.2 Installing Images and De-registering images
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1431
wordpress_url: https://forge.sat.iit.edu/?p=1431
date: '2015-06-22 23:40:29 -0500'
date_gmt: '2015-06-23 04:40:29 -0500'
categories:
- Cloud
tags:
- Eucalyptus
- HP
comments: []
---
Since Eucalyptus 4.0.2 adding images and deregistering images has become vastly simple.  Before hand it seemed as a mystery how to create custom images. 

The secret is in this repo <a href="https://github.com/SAT-Hadoop/cloud-images">https://github.com/SAT-Hadoop/cloud-images</a>  Basically you use <a href="http://packer.io">Packer.io</a> to build your custom images and then use euca-install-image:

```euca-install-image -i output-qemu/centos-6-base.raw --virtualization-type hvm -b centos-base -r x86_64 --name centos-base```

Using this repo I have added the Ubuntu 14.04.2 image and removed 12.04.5 images, as well as the Fedora image.

You can use the commands here: euca-deregister  and euca-delete-bundle <a href="https://www.eucalyptus.com/docs/eucalyptus/4.0.2/index.html#image-guide/img_task_remove_image.html">https://www.eucalyptus.com/docs/eucalyptus/4.0.2/index.html#image-guide/img_task_remove_image.html</a>

Find the image you want to remove. 
```euca-describe-images```

> IMAGE   emi-E533392E    alpha/centos.5-3.x86-64.img.manifest.xml    965590394582
available   public   i386    machine eki-345135C9    eri-C4F135BC  instance-store
IMAGE   emi-623C38B0  alpha/ubuntu.9-04.x86-64.img.manifest.xml   965590394582
available   public   i386    machine eki-E6B13926    eri-94DB3AB9  instance-store

Note the image file name (for example, emi-623C38B0).
Deregister the image. 

```euca-deregister emi-623C38B0```
```IMAGE   emi-623C38B0```

Delete the bundle:

```euca-delete-bundle -b alpha -p ubuntu.9-04.x86-64.img```
