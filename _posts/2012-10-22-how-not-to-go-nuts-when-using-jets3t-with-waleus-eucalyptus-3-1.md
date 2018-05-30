---
layout: post
status: publish
published: true
title: How not to go nuts when using JetS3t with Waleus & Eucalyptus 3.1
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 438
wordpress_url: http://blog.sat.iit.edu/?p=438
date: '2012-10-22 10:20:15 -0500'
date_gmt: '2012-10-22 15:20:15 -0500'
categories:
- tools
- Projects
- Cloud
- fun
tags:
- Eucalyptus
- HP
- Ubuntu
comments: []
---
<p>The open source Cloud Computing Platform <a href="http://www.eucalyptus.com" title="Eucalyptus">Eucalyptus</a> has a neat feature called Walrus or W3.  It is a <a href="http://en.wikipedia.org/wiki/SaaS" title="SaaS">Storage as a Service</a>  compatible with <a href="http://en.wikipedia.org/wiki/Amazon_S3" title="S3">Amazons S3</a> -- but for use in-house.  </p>
<p> I have been using an opensource library called <a href="http://jets3t.s3.amazonaws.com/index.html" title="JetS3t">JetS3t (pronounced JetSet)</a> configured to be able to work with Amazon S3 - <a href="http://en.wikipedia.org/wiki/Amazon_S3" title="Google Storage Servcice">Google Storage Service</a> - and Eucalyptus.  The image below shows the configuration file for JetS3t.</p>
<p>There are a few settings that need changing</p>
<ul>
<code>s3service.https-only      needs to be set to false (no https yet for Walrus)<br />
s3service.s3-endpoint     needs to be set to the IP address of your Walrus instance<br />
s3service.s3-endpoint-virtual-path    needs to be set to the Walrus service location - which is /services/Walrus (no trailing slash)</ul></code><br />
If you change these three you will be able to list your buckets but not objects - hmm...  what is the issue.<br />
There is one more setting that needs to be changed...</p>
<ul>
s3.service-disable-dns-buckets      needs to be set to true<br />
</ul><br />
I beleive this allows the JetS3t library to look up bucket names via direct IP instead of using a FQDN.</p>
<p>Now the next post is how to get the Amazon Java SDK to work with Eucalyptus - this should point me in the right direction.  NOTE - this configuration should work for older Walruses as well.<br />
<a href="http://blog.sat.iit.edu/wp-content/uploads/2012/11/EucalyptusJetS3t.png"><img src="http://blog.sat.iit.edu/wp-content/uploads/2012/11/EucalyptusJetS3t-300x139.png" alt="" title="EucalyptusJetS3t" width="300" height="139" class="alignnone size-medium wp-image-459" /></a></p>
