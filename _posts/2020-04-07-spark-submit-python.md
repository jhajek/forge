---
layout: post
status: publish
published: true
title: 'Spark-Submit Python Code'
author:
  display_name: Jeremy Hajek
  login: jhajek
  email: hajek@.iit.edu
author_login: jhajek
author_email: hajek@.iit.edu
date: '2020-04-07 20:55:52 -0600'
date_gmt: '2020-04-07 12:55:52 -0600'
categories:
- software
tags: 
- hadoop
- spark
- python
- spark-submit
comments: []
---

Working with Python code for Big Data processing on Spark Clusters is a wonderful thing.   The books available are numerous and articles and documentation are prevalent. The only issue I have encountered is that sample code and samples for running Python code on a Spark Cluster is scarce.  

Why?

This happens because setting up a Hadoop [YARN](https://hadoop.apache.org/docs/r2.9.2/hadoop-yarn/hadoop-yarn-site/YARN.html "Link to Apache YARN") cluster requires multiple servers, networking, perhaps a VPN, and configuring software.   Logically then details are sparse.

But not to fear.   Looking at the examples in the 2018 book, [Spark: The Definitive Guide Big Data Processing Made Simple](http://shop.oreilly.com/product/0636920034957.do?afsrc=1 "Spark: The Definitive Guide Big Data Processing Made Simple book") there is sample code that looks like this:

```python
# In Python Page 228 of E-book
from __future__ import print_function
if __name__ == 'main':
    from pyspark.sql import SparkSession
    spark = SparkSession.builder.appName("Demo Spark Python Cluster Program").getOrCreate()

    df = spark.read.csv("hdfs://namenode/user/controller/ncdc-parsed-csv/20/part-r-00000").option("inferSchema","true").option("header","true")
    print(df.show(10))
```

This looks good, but doesn't work...  which is frustrating... because the syntax is correct--to a point.  