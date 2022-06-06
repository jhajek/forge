---
layout: post
status: publish
published: true
title: 500 Error/WordPress upload permissions on IIS 7 Fix
author:
  display_name: lmchughi
  login: lmchughi
  email: lmchughi@iit.edu
  url: ''
author_login: lmchughi
author_email: lmchughi@iit.edu
date: '2013-01-02 12:02:11 -0600'
date_gmt: '2013-01-02 18:02:11 -0600'
tags: []
comments: []
---
<p>Source: <a href="http://chris.wastedhalo.com/2011/01/wordpress-upload-permissions-on-iis-7-fix/" target="_blank">http://chris.wastedhalo.com/2011/01/wordpress-upload-permissions-on-iis-7-fix/ </a></p>
<p><strong>This article will explain how to fix the permission problem on uploaded files in WordPress running on IIS 7.</strong></p>
<p>I was having the same problem as you.┬&aacute; You upload an image in WordPress and either you get an error or the image will upload, thumbnails would work but the actual image would not have read permissions. I&Gamma;&Ccedil;&Ouml;m not an IIS or WordPress expert but after a ton of searching and trying things that didn&Gamma;&Ccedil;&Ouml;t work, this is what worked for me.</p>
<p>If you can&Gamma;&Ccedil;&Ouml;t upload an image at all, it&Gamma;&Ccedil;&Ouml;s probably because you need to give the <strong>IUSR</strong>┬&aacute;account Read/Write/Modify permission on your <strong>wp-content</strong> folder.┬&aacute; This will allow you to upload, and do the WordPress &amp; plugin updates.</p>
<p><a href="http://chris.wastedhalo.com/wp-content/uploads/2011/01/wp-content-permissions.jpg"><img title="wp-content-permissions" alt="" src="http://chris.wastedhalo.com/wp-content/uploads/2011/01/wp-content-permissions.jpg" width="437" height="367" /></a></p>
<p>Once you have done that, all you need to do is give the <strong>IIS_IUSRS</strong> group Read permissions on your <strong>&Gamma;&Ccedil;&pound;C:\Windows\Temp&Gamma;&Ccedil;&yen;</strong> folder.</p>
<p>Make sure to notice that the two permission changes you make are not for the same user/group.┬&aacute;┬&aacute; Give <strong>IUSR</strong> permissions on your <strong>wp-content</strong> folder and <strong>IIS_IUSRS</strong> permissions on your Windows <strong>temp</strong> folder.</p>
<p>I read some other posts saying to edit your php.ini file and change the upload temp directory and give that directory permissions but that didn&Gamma;&Ccedil;&Ouml;t work for me.</p>
<p><a href="http://chris.wastedhalo.com/wp-content/uploads/2011/01/temp-permissions.jpg"><img title="temp-permissions" alt="" src="http://chris.wastedhalo.com/wp-content/uploads/2011/01/temp-permissions.jpg" width="487" height="365" /></a></p>
<p>That should do it, or at least it worked for me.</p>
<p>If you&Gamma;&Ccedil;&Ouml;re still having problems, here is something else you can try.┬&aacute; When I was having problems with my uploaded images I noticed that even though IUSR had permissions on my uploads folder they weren&Gamma;&Ccedil;&Ouml;t propagating to the newly uploaded files.┬&aacute; The other thing I noticed was that when a new image was uploaded the Owner of the new file was IUSR, and instead of giving IUSR the permissions that I had assigned for the parent folder it seemed to copy the permissions from &Gamma;&Ccedil;&pound;CREATOR OWNER&Gamma;&Ccedil;&yen; and assign them to IUSR for the upladed┬&aacute;file.┬&aacute; &Gamma;&Ccedil;&pound;CREATOR OWNER&Gamma;&Ccedil;&yen; doesn&Gamma;&Ccedil;&Ouml;t really have any rights so the next thing I was going to try is to give &Gamma;&Ccedil;&pound;CREATOR OWNER&Gamma;&Ccedil;&yen; Read/Write/Modify permissions on my uploads folder.┬&aacute; Maybe these new permissions would get copied and assigned to IUSR for new uploads?┬&aacute; Worth a try anyway.</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
