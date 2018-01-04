---
layout: post
status: publish
published: true
title: Using S3cmd with Walrus and HP Eucalyptus 4.0.2 works but with a twist...
author:
  display_name: hajek
  login: hajek
  email: hajek@iit.edu
  url: ''
author_login: hajek
author_email: hajek@iit.edu
wordpress_id: 1741
wordpress_url: https://forge.sat.iit.edu/?p=1741
date: '2016-05-31 00:16:34 -0500'
date_gmt: '2016-05-31 05:16:34 -0500'
categories:
- Cloud
- jeremy
- Eucalyptus
tags:
- cloud
- Eucalyptus
- S3cmd
comments: []
---
Dear Team,

I would like to pass on that using S3cmd works fine with Eucalyptus 4.0.2 (will be installing and testing 4.2.2 soon) with a twist...   Assuming you are using Linux or the Bash on Windows 10 (Linux subsystem) for this exercise...

[s3cmd](https://github.com/s3tools/s3cmd "S3cmd") is a commandline tool built in Python for accessing Object Storage via the commandline.  This is useful for [AWS S3](https://aws.amazon.com/s3/ "AWS S3"), but also for [HP Eucalyptus Walrus](http://www8.hp.com/us/en/cloud/helion-eucalyptus.html "HP Eucalyptus Walrus") (Eucalyptus S3 Object storage--AWS compatible).

![*S3cmd Permission Denied*](/assets/2016/05/s3cmd-permission-denied.png)

*My command* was as follows: ```./s3cmd -v -c ./s3cfg la s3://vagrant-builds```
A bucket I created with my account credentials should have list and read permission.  Even switching to the ADMIN credentials the same error appears.

Then I noticed a little tidbit in the S3cmd --help file that said this:
![*Signature V2*](/assets/2016/05/signature-v2.png "signature-v2")

Look at the entry that says: 
``` --signature-v2 ```    Use AWS Signature version 2 instead of newer signature methods. 

Helpful for S3-like systems that don't have AWS Signature v4 yet.
![*Jinkies*](/assets/2016/05/velma_jinkies_by_tinent.png)

Thats right Velma, *jinkees*, a clue. AWS changes their API often and certain aspects of their code changes that invalidates using old APIs--which I guess makes sense from their upkeep point of view.  BUT it makes it hard for honest Eucalyptus developers that are trying to peg their product to AWS Java SDK, so it seems they are a few signature versions back.

**How to generate s3cfg**
These are detailed links to S3cmd and its setup.

* [https://support.eucalyptus.com/hc/en-us/articles/202810049-Using-s3cmd-with-Eucalyptus](https://support.eucalyptus.com/hc/en-us/articles/202810049-Using-s3cmd-with-Eucalyptus)
* [https://kushaldas.in/posts/s3cmd-and-walrus.html](https://kushaldas.in/posts/s3cmd-and-walrus.html)

Copy the text below and place it in a file named .s3cfg in your home directory.
```[default]
access_key = YOURKEYHERE
secret_key = YOURSECRETKEYHERE
host_base = xxx.xxx.xxx.xxx:8773
host_bucket = xxx.xxx.xxx.xxx:8773
service_path = /services/objectstorage
bucket_location = US
default_mime_type = binary/octet-stream
delete_removed = False
dry_run = False
enable_multipart = True
encoding = UTF-8
encrypt = False
follow_symlinks = False
force = False
get_continue = False
gpg_command = /usr/bin/gpg
gpg_decrypt = %(gpg_command)s -d --verbose --no-use-agent --batch --yes --passphrase-fd %(passphrase_fd)s -o %(output_file)s %(input_file)s
gpg_encrypt = %(gpg_command)s -c --verbose --no-use-agent --batch --yes --passphrase-fd %(passphrase_fd)s -o %(output_file)s %(input_file)s
gpg_passphrase = password
guess_mime_type = True
human_readable_sizes = False
invalidate_on_cf = False
list_md5 = False
log_target_prefix =
mime_type =
multipart_chunk_size_mb = 15
preserve_attrs = True
progress_meter = True
proxy_host =
proxy_port = 0
recursive = False
recv_chunk = 4096
reduced_redundancy = False
send_chunk = 4096
skip_existing = False
socket_timeout = 300
urlencoding_mode = normal
use_https = False
verbosity = WARNING
```

Adding this flag to the S3cmd solves the long listing problem using my own credentials.
```./s3cmd -v --signature-v2 -c ./s3cfg la s3://vagrant-builds```

Now the results are what I was looking for!
![*S3cmd-works*](/assets/2016/05/s3cmd-works.png)
