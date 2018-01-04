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
![*HPE Eucalyptus*](/assets/2016/06/225px-Eucalyptus-Logo.jpg)

<strike>Here at Illinois Tech we</strike> I have just completed a successful semester of using HP Eucalyptus 4.0.2 Cloud Platform for all of our capstone and research projects.  It has been a blessing as it has taken hardware requests and budgeting problems out of the IT groups hands and placed it into the students hands.  They were able to rapidly deploy their services, load-balancers and any resources needed on their own.

Still in the process of upgrading to Eucalyptus 4.0.2 but it still works with S3cmd and the AWS Java API (as they intended it to).

This article is about Eucalyptus 4.0.2 -- I still haven't done the upgrade, 4.2.2 adds a few new features (looking at you [Eucalyptus Architecture for UFS](http://docs.hpcloud.com/eucalyptus/4.2.2/#install-guide/euca_architecture.html "Eucalyptus Architecture") that I need to build in a test environment.  I also need to upgrade my old HP DL 360s which have been work horses but the memory is only 4GB so that is asking for trouble.

This article is about getting the AWS Java SDK working with Eucalyptus 4.0.2.  I was able to make this work... with conditions.  The first issue is to review current information on hand.  A quick search of the old Eucalyptus GitHub wiki gives some clues but only talks about ASW Java SDK version 1.3.x and Eucalyptus version 3.3.x.  Of use for a few clues.

The next place to search is the [Eucalyptus Users Google Group](https://groups.google.com/a/eucalyptus.com/forum/#!forum/euca-users "Eucalyptus Users Google Group"), which I recommend to visit if you have a Eucalyptus issue, very friendly people who love the product.  I executed this search string: [https://groups.google.com/a/eucalyptus.com/forum/#!searchin/euca-users/AWS$20Java$20SDK](https://groups.google.com/a/eucalyptus.com/forum/#!searchin/euca-users/AWS$20Java$20SDK) and a few interesting results came up. (Note one of the questions is my own...)

![*AWS Compat List*](/assets/2016/06/aws-java-compat-list-768x403.png)

Credit goes to HP Eucalyptus employee, **Steve Jones** and user **CoderSparks**, who noticed that Walrus doesn't support signature v4 in the AWS Java SDK -- you need to default back to the version 2 signature -- just like in the previous [S3cmd article](https://forge.sat.iit.edu/2016/05/using-s3cmd-with-walrus-and-hp-eucalyptus-4-0-2-works-but-with-a-twist/)

I wanted to see for myself what the issue was so I tried to use the latest AWS Java SDK v1.11.4.  Surprisingly everything worked, but one important feature. Let's find out why.

**Development Environment**
My setup was:

* Windows 10 machine
*  <a href="https://netbeans.org/">Netbeans 8.1</a>
*  <del datetime="2016-07-14T03:31:32+00:00"><a href="https://github.com/aws/aws-sdk-java">AWS Java SDK GitHub repo </a>(contains the S3Sample file and Maven project build files)</del>
*  EDIT 07/13/16 - Clone this project: <a href="https://github.com/awslabs/aws-java-sample.git">https://github.com/awslabs/aws-java-sample.git</a>

**--EDIT 06/16/16--**

<del datetime="2016-06-16T21:00:18+00:00">I opened the sample/test code provided by Amazon, located in the <a href="https://github.com/aws/aws-sdk-java/tree/master/src/samples/AmazonS3">https://github.com/aws/aws-sdk-java/blob/master/src/samples/AmazonS3/S3Sample.java</a> file.</del>

1) start by cloning the AWS Java SDK sample projects from AWSlab: 
  +  ```git clone https://github.com/awslabs/aws-java-sample.git```

2) you can then open the project directly in NetBeans and it will import the entire build package (S3Sample.java) plus the Maven file (pom.xml) required to download all the .jar dependencies. Just hit the green RUN triangle and Maven will retrieve all of the JARs needed, even the AWS Java SDK.

3) **Issues Encountered**
  +  I encountered one small issue using AWS Java API 1.11.4 -- <a href="https://groups.google.com/a/eucalyptus.com/forum/#!searchin/euca-users/aws$20java$20sdk/euca-users/hnU9NmFM7-Q/W-AScuslW1YJ">this issue is referenced here by member CoderSparks</a>. - though the questioner had a different error -- I think the concept was the same:  

4) **What I Did Next**
  + Let me show you the initial code, my additions, and then explain it below.  Modify the first few lines of code in the sample to reflect below.

{% highlight java %} // My additions to the S3Sample.java code
ClientConfiguration opts = new ClientConfiguration();
opts.setSignerOverride("S3SignerType");  // NOT "AWS3SignerType"  -- mentioned by Steve Jones set signatures back to v2.
AmazonS3Client s3 = new AmazonS3Client(opts);
// Region usWest2 = Region.getRegion(Regions.US_WEST_2);
// s3.setRegion(usWest2);
s3.setEndpoint("http://objectstorage.yourdomain.com:8773/services/objectstorage/");
s3.setS3ClientOptions(new S3ClientOptions().withPathStyleAccess( true ) );
{% endhighlight %}

Also don't forget to create a credential file on the local system--don't put creds in your actual code--too risky. In your home directory (Linux or Windows), create a directory named **.aws** with a file named **credentials** inside that file place the syntax below.  These keys can be AWS account keys or HP Eucalyptus Account keys. The keys will come in your credentials zip file provided by your Eucalyptus Administrator, extract the zip file and the credentials listed below will be contained in a file named **eucarc**. In this story they are your account keys from our HP Eucalyptus system.

```[default]
aws_access_key_id = YOURACCESSKEYHERE
aws_secret_access_key = YOURSECRETKEYHERE
```

This is how the S3 connection object looks in the S3Sample.java provided in the Github link for the AWS JAVA SDK project.

{% highlight java %} AmazonS3 s3 = new AmazonS3Client();
Region usWest2 = Region.getRegion(Regions.US_WEST_2);
s3.setRegion(usWest2);
{% endhighlight %}

Eucalyptus has *different* regions so to speak. So you need to comment out a few AWS specific lines and add an override so the code knows how to find your Walrus/OSG (Object Storage Gateway) URI.
{% highlight java %} AmazonS3 s3 = new AmazonS3Client();
//Region usWest2 = Region.getRegion(Regions.US_WEST_2);
//s3.setRegion(usWest2);
s3.setEndpoint("http://objectstorage.yourdomain.com:8773/services/objectstorage");
s3.setS3ClientOptions(new S3ClientOptions().withPathStyleAccess( true ) );
{% endhighlight %}

With this set, the code will compile and run and connect to AWS Java SDK 1.11.4, and all of the JAR dependencies will be fetched by <a href="https://en.wikipedia.org/wiki/Apache_Maven">Apache Maven</a> for you, easy setup no need to download any additional JARs or classpath. 

**From Whence Came That Error?**
I am glad you asked. I though it might be <a href="https://forge.sat.iit.edu/2016/05/using-s3cmd-with-walrus-and-hp-eucalyptus-4-0-2-works-but-with-a-twist/">the same issue as the in the S3cmd article...</a>  Let's look at the sample code and go item by item until we get to the error and squeeze its meaning out.


*  Proper Connection being made to Eucalyptus OSG (Walrus) ![](/assets/2016/05/Green-Tick.png)
*  Proper code that creates a new bucket ![](/assets/2016/05/Green-Tick.png)
*  Proper code that creates uploads an new object to the previously created bucket ![](/assets/2016/05/Green-Tick.png)
*   Proper code that downloads that previously uploaded object ![](/assets/2016/06/th.jpg)

**Error as follows from the exception handling in Java:**

``` 
Downloading an object
May 31, 2016 12:37:06 AM com.amazonaws.services.s3.AmazonS3Client getSignerByURI
WARNING: Failed to parse the endpoint http://objectstorage.sat.iit.edu:8773/services/objectstorage, and skip re-signing the signer region
java.lang.IllegalArgumentException: Invalid S3 URI: hostname does not appear to be a valid S3 endpoint: http://objectstorage.sat.iit.edu:8773/services/objectstorage
```

![S3 v3 error](/assets/2016/06/sampleS3-v3-error.png)
![S3 v2 error v2](/assets/2016/06/sampleS3-v3-error-P2-905x192.png)
What could be causing this error?  The connection is proper and 3/4ths of the code works! It is the signature problem mentioned above but also something else.  The AWS Java SDK is too new and the way it handles downloading an object is not compatible.  This means I had to roll versions back.  If you roll back to AWS Java SDK 1.9.4 then you get 4 green check marks. So the solution is:

*  Add the setSignature() method detailed above
*  Change the POM.xml file value to pull AWS Java SDK 1.9.4 (no higher)
*  Walrus problems are solved

![*POM Assests*](/assets/2016/06/pom-xml-1-9-4-768x275.png)

Take care, Jeremy!
