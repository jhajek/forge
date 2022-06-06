---
layout: post
status: publish
published: true
title: 'Hadoop 2.8.5 and native extensions now work OOTB'
author:
  display_name: Jeremy Hajek
  login: jhajek
  email: hajek@.iit.edu
author_login: jhajek
author_email: hajek@.iit.edu
date: '2019-01-12 20:55:52 -0600'
date_gmt: '2019-01-12 22:55:52 -0600'
tags: 
- hadoop
- compresssion
- extensions
comments: []
---

In previously working with Hadoop 2.6.5, at that time due to opensource licensing constraints between the [Apache License 1.1](https://en.wikipedia.org/wiki/Apache_License "Apache License 1.1") and the [BSD Licenses](https://en.wikipedia.org/wiki/BSD_licenses "BSD licenses") certain compression libraries were not able to be included in Hadoop.  You had to install the development libraries and [compile the native extensions yourself](https://hadoop.apache.org/docs/r2.6.5/hadoop-project-dist/hadoop-common/NativeLibraries.html "Compile Instructions for hadoop 2.6.5"
) for the 2.6.5 version.

The process was quite straightforward and I was able to do it on multiple occasions.

In transitioning to the 2.8.5 version a quick run of the ```hadoop checknative -a``` command gives this pleasent output (and relieves me of having to compile the native modules).

```bash
vagrant@itmd521:~$ hadoop checknative -a
19/01/12 22:48:41 INFO bzip2.Bzip2Factory: Successfully loaded & initialized native-bzip2 library system-native
19/01/12 22:48:41 INFO zlib.ZlibFactory: Successfully loaded & initialized native-zlib library
Native library checking:
hadoop:  true /home/vagrant/hadoop-2.8.5/lib/native/libhadoop.so.1.0.0
zlib:    true /lib/x86_64-linux-gnu/libz.so.1
snappy:  true /usr/lib/x86_64-linux-gnu/libsnappy.so.1
lz4:     true revision:10301
bzip2:   true /lib/x86_64-linux-gnu/libbz2.so.1
openssl: false EVP_CIPHER_CTX_cleanup
```

Now the last piece is to understand the last error message but he openssl error.  Since I am not using any openssl encryption features I can ignore this for now, but I am curious as to what it means.
