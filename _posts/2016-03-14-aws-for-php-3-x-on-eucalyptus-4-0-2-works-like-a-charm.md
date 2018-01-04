---
layout: post
status: publish
published: true
title: AWS for PHP 3.x on Eucalyptus 4.0.2 - EC2 commands works like a charm
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1659
wordpress_url: https://forge.sat.iit.edu/?p=1659
date: '2016-03-14 14:39:26 -0500'
date_gmt: '2016-03-14 19:39:26 -0500'
categories:
- Cloud
- fun
- Eucalyptus
tags:
- cloud
- HP Eucalyptus 4.0.2
- AWS-SDK-PHP-V3
comments: []
---
Hello,
  My mission started off to see if I could figure out how to integrate the <a href="http://docs.aws.amazon.com/aws-sdk-php/v3/api/">AWS SDK for PHP v3</a> to use it on Eucalyptus 4.0.2 production systems.
Researching the internet turned up not much current information.  But it did turn up these two original pieces of information about getting the AWS PHP SDK v1 to run from 2013.
<ul>
<li><a href="https://github.com/eucalyptus/eucalyptus/wiki/Using-PHP-with-Eucalyptus">https://github.com/eucalyptus/eucalyptus/wiki/Using-PHP-with-Eucalyptus</a></li>
<li><a href="https://github.com/eucalyptus/eucalyptus/wiki/AWS-SDK-for-PHP">https://github.com/eucalyptus/eucalyptus/wiki/AWS-SDK-for-PHP</a></li>
</ul>
But AWS PHP SDK 1.0 is <a href="https://github.com/amazonwebservices/aws-sdk-for-php">deprecated</a>, big time. 
So what to do?  Perhaps you could just make a drop in replacement?  If only that was possible.  It seems that AWS always modifys there API terminology per version.  B
To the rescue! As of Eucalyptus 4.x branch the developers have managed to match their code perfectly and v3 api works right out of the box.
Steps to recreate:
Install the AWS PHP SDK v3 via composer following <a href="http://docs.aws.amazon.com/aws-sdk-php/v3/guide/getting-started/installation.html">normal AWS directions</a>
Once that is done - you need to do some reading.   
Based on the two articles of the hardwork done to make the AWS PHP SDK work with Eucalyptus no longer work directly - you have to read the Aws/Ec2/Ec2Client constructor.  All the information is actually encased in the previous Eucalyptus wiki articles but the constructor names have changed!
So after 10 minutes of reading I realized that this was an easy fix.
<a href="http://docs.aws.amazon.com/aws-sdk-php/v3/api/class-Aws.Ec2.Ec2Client.html">Go directly to the Elastic Compute Cloud portion of the AWS PHP SDK</a> and scroll down to the constructor and see for yourself.

![*EC2 Output*](/assets/2016/03/ec2-768x460.png)

Once that work was done here is all that is needed to get the EC2 tools via PHP running:

{% highlight php %}
require 'vendor/autoload.php';
use Aws\Ec2\Ec2Client;
$client = new Ec2Client([
  'scheme' => 'http',
  'endpoint' => 'Your-CLC-IP-Here:8773/',
  'validate' => false,
  'version' => '2015-10-01',
  'region' => 'us-east-1',
  'credentials' => [
     'key' => 'Your-KEY-HERE',
     'secret' => 'Your-Secret-Key_here',
     ],
]);
$result = $client->describeInstances([
]);
print_r($result);
?>
{% endhighlight %}

Note I hard coded the credentials for two reasons:  1 you have to change the permissions of any credentials file - like ~/.aws/crednetials because it is owned by root:root on any Eucalyptus created image.
Or you could use IAM - which I am working on =) 
But at least this gets you started!
S3 is my next take but their constructor is not so straight forward.
take care
Jeremy Hajek 
