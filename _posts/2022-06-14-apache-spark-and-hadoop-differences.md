---
layout: post
status: publish
published: true
title: 'The difference between Apache Spark and Apache Hadoop'
tags: 
- hadoop
- spark
- big data
comments: []
---

### Apache Spark

[Apache Spark](https://spark.apache.org/ "Apache Spark WebPage") is a "Unified engine for large-scale data analytics." Spark is technically the evolution of the [Apache Hadoop](https://hadoop.apache.org "Apache Hadoop WebSite") platform.

Spark started development and released as a product ~2012. Spark analyzed what Hadoop had done and the design decisions that had been made during its creation.

The main difference is that Spark decided not to provide a storage engine. Hadoop included its own specialized file called [HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html "HDFS documentation page"). This was done at a time when filesystems on Linux and Unix had not fully matured. We were still dealing with [ext3](https://en.wikipedia.org/wiki/Ext3 "Ext3 wiki page") and [ext4](https://en.wikipedia.org/wiki/Ext4 "Ext4 wiki page") on Linux. These file systems were engineered for smaller file sizes and the random nature of a PC Operating System.

Hadoop was designed in the mid to late 2000's. You have to remember the at that time the size and speed of hard drives at that time period. HDFS was required at that time. Spark came to realize that files and filesystems had expanded beyond local disk.

Around 2010 as Cloud and [Object based storage](https://en.wikipedia.org/wiki/Object_storage "Object Storage wiki page"), such as [S3](https://aws.amazon.com/s3/ "AWS S3 webpage"), begin to grow you began to see why Spark switched tactics. In addition, people began to realize that [SQL](https://en.wikipedia.org/wiki/SQL "SQL wiki page") was actually a pretty good language to get results from data. So Spark decided to add native libraries to allow your application to interface directly with storage: [S3](https://aws.amazon.com/s3/ "AWS S3 webpage") as well as databases such as [MySQL](https://dev.mysql.com/ "MySQL web page")/[MariaDB](https://mariadb.org/ "MariaDB web page"), [Postgres](https://www.postgresql.org/ "Postgres web page"), or any other database via [JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity "JDBC wiki page")/[ODBC](https://en.wikipedia.org/wiki/Open_Database_Connectivity "ODBC wikipage").

This opened up opportunities to keep your data in its original place. The down side is that you have to transfer your data over a network to your Spark cluster. As data scales this becomes impossible over the public network. The solution is to place your Spark cluster next to your database or your S3 storage. That could mean using a Cloud Service or having your own on-prem cluster placed next to a local S3-like storage cluster -- say [Minio](https://min.io "Minio web page").

## Conclusion

Spark has many advantages and is easy to setup. If you have extra hardware or equipment you could run a cluster quite easily on premises.  Using on-prem Object storage using Minio you can then setup local Big Data Storage.
