---
layout: post
status: publish
published: true
title: Eucalyptus 4.0.2 supports using AWS Java SDK 1.11.4 for Walrus but really 1.9.4
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1737
wordpress_url: https://forge.sat.iit.edu/?p=1737
date: '2016-06-03 23:47:23 -0500'
date_gmt: '2016-06-04 04:47:23 -0500'
categories:
- Uncategorized
- tools
- Cloud
- Eucalyptus
tags:
- Eucalyptus
- HP Eucalyptus
- teaching
- AWS Java SDk
comments: []
---
<p><strong><a href="/assets/2016/06/225px-Eucalyptus-Logo.jpg"><img src="/assets/2016/06/225px-Eucalyptus-Logo.jpg" alt="225px-Eucalyptus-Logo" width="225" height="24" class="aligncenter size-full wp-image-1760" /></a></p>
<p>   Here at Illinois Tech we have just completed a successful semester of using HP Eucalyptus 4.0.2 Cloud Platform for all of our capstone and research projects.  It has been a blessing as it has taken hardware requests and budgeting problems out of the IT groups hands and placed it into the students hands.  They were able to rapidly deploy their services, load-balancers and any resources needed on their own.</p>
<p>Still in the process of upgrading to Eucalyptus 4.0.2 but it still works with S3cmd and the AWS Java API (as they intended it to).  </p>
<p>This article is about Eucalyptus 4.0.2 -- I still haven't done the upgrade, 4.2.2 adds a few new features (looking at you <a href="http://docs.hpcloud.com/eucalyptus/4.2.2/#install-guide/euca_architecture.html">UFS</a> that I need to build in a test environment.  I also need to upgrade my old HP DL 360s which have been work horses but the memory is only 4GB so that is asking for trouble. </p>
<p>This article is about getting the AWS Java SDK working with Eucalyptus 4.0.2.  I was able to make this work... with conditions.  The first issue is to review current information on hand.  A quick search of the old Eucalyptus GitHub wiki gives some clues but only talks about ASW Java SDK version 1.3.x and Eucalyptus version 3.3.x.  Of use for a few clues.</p>
<p>The next place to search is the <a href="https://groups.google.com/a/eucalyptus.com/forum/#!forum/euca-users">Eucalyptus Users Google Group</a>, which I recommend to visit if you have a Eucalyptus issue, very friendly people who love the product.  I executed this search string: <a href="https://groups.google.com/a/eucalyptus.com/forum/#!searchin/euca-users/AWS$20Java$20SDK">https://groups.google.com/a/eucalyptus.com/forum/#!searchin/euca-users/AWS$20Java$20SDK</a> and a few interesting results came up. (Note one of the questions is my own...)</p>
<p><a href="/assets/2016/06/aws-java-compat-list.png"><img src="/assets/2016/06/aws-java-compat-list-300x157.png" alt="aws-java-compat-list" width="300" height="157" class="aligncenter size-medium wp-image-1772" /></a></p>
<p> Credit goes to HP Eucalyptus employee, <strong>Steve Jones</strong> and user <strong>CoderSparks</strong>, who noticed that Walrus doesn't support signature v4 in the AWS Java SDK -- you need to default back to the version 2 signature -- just like in the previous <a href="https://forge.sat.iit.edu/2016/05/using-s3cmd-with-walrus-and-hp-eucalyptus-4-0-2-works-but-with-a-twist/">S3cmd article</a>. </p>
<p>I wanted to see for myself what the issue was so I tried to use the latest AWS Java SDK v1.11.4.  Surprisingly everything worked, but one important feature. Let's find out why.</p>
<p><strong>Development Environment</strong></p>
<p>My setup was:</p>
<ul>
<li>Windows 10 machine</li>
<li><a href="https://netbeans.org/">Netbeans 8.1</a></li>
<li><del datetime="2016-07-14T03:31:32+00:00"><a href="https://github.com/aws/aws-sdk-java">AWS Java SDK GitHub repo </a>(contains the S3Sample file and Maven project build files)</del></li>
<li>EDIT 07/13/16 - Clone this project: <a href="https://github.com/awslabs/aws-java-sample.git">https://github.com/awslabs/aws-java-sample.git</a></li>
<p><strong>--EDIT 06/16/16--</strong><br />
<del datetime="2016-06-16T21:00:18+00:00">I opened the sample/test code provided by Amazon, located in the <a href="https://github.com/aws/aws-sdk-java/tree/master/src/samples/AmazonS3">https://github.com/aws/aws-sdk-java/blob/master/src/samples/AmazonS3/S3Sample.java</a> file.</del> </p>
<p><strong>First</strong> start by cloning the AWS Java SDK sample projects from AWSlab: </p>
<p><code>git clone https://github.com/awslabs/aws-java-sample.git</code></p>
<p><strong>Second</strong> you can then open the project directly in NetBeans and it will import the entire build package (S3Sample.java) plus the Maven file (pom.xml) required to download all the .jar dependencies. Just hit the green RUN triangle and Maven will retrieve all of the JARs needed, even the AWS Java SDK.</p>
<p><strong>Issues Encountered</strong><br />
I encountered one small issue using AWS Java API 1.11.4 -- <a href="https://groups.google.com/a/eucalyptus.com/forum/#!searchin/euca-users/aws$20java$20sdk/euca-users/hnU9NmFM7-Q/W-AScuslW1YJ">this issue is referenced here by member CoderSparks</a>. - though the questioner had a different error -- I think the concept was the same:   </p>
<p><strong>What I Did Next</strong><br />
Let me show you the initial code, my additions, and then explain it below.  Modify the first few lines of code in the sample to reflect below.</p>
<pre lang="Java">// My additions to the S3Sample.java code<br />
ClientConfiguration opts = new ClientConfiguration();<br />
opts.setSignerOverride("S3SignerType");  // NOT "AWS3SignerType"  -- mentioned by Steve Jones set signatures back to v2.</p>
<p>AmazonS3Client s3 = new AmazonS3Client(opts);<br />
// Region usWest2 = Region.getRegion(Regions.US_WEST_2);<br />
// s3.setRegion(usWest2);<br />
s3.setEndpoint("http://objectstorage.yourdomain.com:8773/services/objectstorage/");<br />
s3.setS3ClientOptions(new S3ClientOptions().withPathStyleAccess( true ) );<br />
</pre></p>
<p>Also don't forget to create a credential file on the local system--don't put creds in your actual code--too risky. In your home directory (Linux or Windows), create a directory named <strong>.aws</strong> with a file named <strong>credentials</strong> inside that file place the syntax below.  These keys can be AWS account keys or HP Eucalyptus Account keys. The keys will come in your credentials zip file provided by your Eucalyptus Administrator, extract the zip file and the credentials listed below will be contained in a file named <strong>eucarc</strong>. In this story they are your account keys from our HP Eucalyptus system.</p>
<p><code>[default]<br />
aws_access_key_id = YOURACCESSKEYHERE<br />
aws_secret_access_key = YOURSECRETKEYHERE<br />
</code></p>
<p>This is how the S3 connection object looks in the S3Sample.java provided in the Github link for the AWS JAVA SDK project.</p>
<pre lang="Java">AmazonS3 s3 = new AmazonS3Client();<br />
Region usWest2 = Region.getRegion(Regions.US_WEST_2);<br />
s3.setRegion(usWest2);<br />
</pre></p>
<p>Eucalyptus has <em>different</em> regions so to speak. So you need to comment out a few AWS specific lines and add an override so the code knows how to find your Walrus/OSG (Object Storage Gateway) URI.</p>
<pre lang="Java">AmazonS3 s3 = new AmazonS3Client();<br />
//Region usWest2 = Region.getRegion(Regions.US_WEST_2);<br />
//s3.setRegion(usWest2);<br />
s3.setEndpoint("http://objectstorage.yourdomain.com:8773/services/objectstorage");<br />
s3.setS3ClientOptions(new S3ClientOptions().withPathStyleAccess( true ) );<br />
</pre></p>
<p>With this set, the code will compile and run and connect to AWS Java SDK 1.11.4, and all of the JAR dependencies will be fetched by <a href="https://en.wikipedia.org/wiki/Apache_Maven">Apache Maven</a> for you, easy setup no need to download any additional JARs or classpath. </p>
<p><strong>From Whence Came That Error?</strong><br />
I am glad you asked. I though it might be <a href="https://forge.sat.iit.edu/2016/05/using-s3cmd-with-walrus-and-hp-eucalyptus-4-0-2-works-but-with-a-twist/">the same issue as the in the S3cmd article...</a>  Let's look at the sample code and go item by item until we get to the error and squeeze its meaning out.</p>
<ul>
<li>Proper Connection being made to Eucalyptus OSG (Walrus) <a href="/assets/2016/05/Green-Tick.png"><img src="/assets/2016/05/Green-Tick.png" alt="Green-Tick" width="20" height="17" class="alignnone size-full wp-image-1752" /></a></li>
<li>Proper code that creates a new bucket <a href="/assets/2016/05/Green-Tick.png"><img src="/assets/2016/05/Green-Tick.png" alt="Green-Tick" width="20" height="17" class="alignnone size-full wp-image-1752" /></a></li>
<li>Proper code that creates uploads an new object to the previously created bucket <a href="/assets/2016/05/Green-Tick.png"><img src="/assets/2016/05/Green-Tick.png" alt="Green-Tick" width="20" height="17" class="alignnone size-full wp-image-1752" /></a></li>
<li> Proper code that downloads that previously uploaded object <a href="/assets/2016/06/th.jpg"><img src="/assets/2016/06/th.jpg" alt="th" width="20" height="20" class="alignnone size-full wp-image-1759" /></a></li><br />
</ul></p>
<p><strong>Error as follows from the exception handling in Java: </strong></p>
<p><code>Downloading an object<br />
May 31, 2016 12:37:06 AM com.amazonaws.services.s3.AmazonS3Client getSignerByURI<br />
WARNING: Failed to parse the endpoint http://objectstorage.sat.iit.edu:8773/services/objectstorage, and skip re-signing the signer region<br />
java.lang.IllegalArgumentException: Invalid S3 URI: hostname does not appear to be a valid S3 endpoint: http://objectstorage.sat.iit.edu:8773/services/objectstorage<br />
</code></p>
<p><a href="/assets/2016/06/sampleS3-v3-error.png"><img src="/assets/2016/06/sampleS3-v3-error.png" alt="sampleS3-v3-error" width="942" height="214" class="aligncenter size-full wp-image-1763" /></a></p>
<p><a href="/assets/2016/06/sampleS3-v3-error-P2.png"><img src="/assets/2016/06/sampleS3-v3-error-P2.png" alt="sampleS3-v3-error-P2" width="916" height="192" class="aligncenter size-full wp-image-1764" /></a></p>
<p>What could be causing this error?  The connection is proper and 3/4ths of the code works </p>
<p>It is the signature problem mentioned above but also something else.  The AWS Java SDK is too new and the way it handles downloading an object is not compatible.  This means I had to roll versions back.  If you roll back to AWS Java SDK 1.9.4 then you get 4 green check marks.</p>
<p>TL DR; Solution</p>
<ul>
<li>Add the setSignature() method detailed above</li>
<li>Change the POM.xml file value to pull AWS Java SDK 1.9.4 (no higher)</li>
<li>Walrus problems are solved</li><br />
</ul></p>
<p><a href="/assets/2016/06/pom-xml-1-9-4.png"><img src="/assets/2016/06/pom-xml-1-9-4-300x107.png" alt="pom-xml-1-9-4" width="300" height="107" class="alignnone size-medium wp-image-1765" /></a></p>
<p>Take care<br />
Jeremy!</p>
