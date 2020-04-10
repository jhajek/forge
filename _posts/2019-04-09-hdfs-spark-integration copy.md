---
layout: post
status: publish
published: true
title: 'Spark/HDFS Integration Sample'
author:
  display_name: Jeremy Hajek
  login: jhajek
  email: hajek@.iit.edu
author_login: jhajek
author_email: hajek@.iit.edu
date: '2019-04-09 20:55:52 -0600'
date_gmt: '2019-04-09 22:55:52 -0600'
categories:
- software
tags: 
- hadoop
- spark
- hdfs
comments: []
---

```scala

val data = spark.read.option("delimiter","\t").option("header","true").csv("hdfs:///ncdc/sample2.txt")
```

This sample taken from [Stackoverflow Question 37638618](https://stackoverflow.com/questions/37638618 "Using Spark Dataframe to load datta from hdfs")
