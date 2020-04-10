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

This looks good, but doesn't work...  which is frustrating... because the syntax is correct--to a point. To run this code save the file as main.py and run this command:

```bash
 spark-submit --verbose --master yarn --deploy-mode cluster demo-read-fifth/demo-read.py
 ```

 The results are a bit strange in YARN:

```bash
20/04/10 03:40:48 INFO yarn.ApplicationMaster: Starting the user application in a separate Thread
20/04/10 03:40:48 INFO yarn.ApplicationMaster: Waiting for spark context initialization...
20/04/10 03:40:48 INFO yarn.ApplicationMaster: Final app status: FAILED, exitCode: 13
20/04/10 03:40:48 ERROR yarn.ApplicationMaster: Uncaught exception:
java.lang.IllegalStateException: User did not initialize spark context!
	at org.apache.spark.deploy.yarn.ApplicationMaster.runDriver(ApplicationMaster.scala:486)
	at org.apache.spark.deploy.yarn.ApplicationMaster.org$apache$spark$deploy$yarn$ApplicationMaster$$runImpl(ApplicationMaster.scala:305)
	at org.apache.spark.deploy.yarn.ApplicationMaster$$anonfun$run$1.apply$mcV$sp(ApplicationMaster.scala:245)
	at org.apache.spark.deploy.yarn.ApplicationMaster$$anonfun$run$1.apply(ApplicationMaster.scala:245)
	at org.apache.spark.deploy.yarn.ApplicationMaster$$anonfun$run$1.apply(ApplicationMaster.scala:245)
	at org.apache.spark.deploy.yarn.ApplicationMaster$$anon$3.run(ApplicationMaster.scala:780)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:422)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1698)
	at org.apache.spark.deploy.yarn.ApplicationMaster.doAsUser(ApplicationMaster.scala:779)
	at org.apache.spark.deploy.yarn.ApplicationMaster.run(ApplicationMaster.scala:244)
	at org.apache.spark.deploy.yarn.ApplicationMaster$.main(ApplicationMaster.scala:804)
	at org.apache.spark.deploy.yarn.ApplicationMaster.main(ApplicationMaster.scala)
```

The main focus is this one line that says **ERROR**:

```bash
0/04/10 03:40:48 ERROR yarn.ApplicationMaster: Uncaught exception: java.lang.IllegalStateException: User did not initialize spark context!
```

So it turns out there is a syntax error in the code that triggers the application to crash.  But since this application is running on the cluster -- submitted to the cluster -- there is no application output or error log.  You are stuck with the YARN application log, which just tells you that the program in the AM container has failed and was retried twice, then the entire job was killed. What to do?  The phrase *spark context* references an older version of Spark (v1.x) way of creating a context object, but that has been superseded in Spark 2.x by using the `SparkSession` object.

The solution?

Taking a look at [Pyspark in Action MEAP](https://www.manning.com/books/pyspark-in-action "Pyspark in Action MEAP book webpage") and the sample code from chapter 03 gives us a hint what the problem might be.

```python
from pyspark.sql import SparkSession
import pyspark.sql.functions as F


spark = SparkSession.builder.appName(
    "Counting word occurences from a book."
).getOrCreate()

spark.sparkContext.setLogLevel("WARN")

# If you need to read multiple text files, replace `1342-0` by `*`.
results = (
    spark.read.text("./data/Ch02/1342-0.txt")
    .select(F.split(F.col("value"), " ").alias("line"))
    .select(F.explode(F.col("line")).alias("word"))
    .select(F.lower(F.col("word")).alias("word"))
    .select(F.regexp_extract(F.col("word"), "[a-z']*", 0).alias("word"))
    .where(F.col("word") != "")
    .groupby(F.col("word"))
    .count()
)

results.orderBy("count", ascending=False).show(10)
results.coalesce(1).write.csv("./results_single_partition.csv")
```

What is the solution?  Note that Python spaces/tabs have meaning as well as main method declaration.  Taking these adjustments into consideration you now have this working code:

```python
# In Python Page 228 of E-book
from __future__ import print_function

from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("Demo Spark Python Cluster Program").getOrCreate()

df = spark.read.format("csv").option("inferSchema","true").option("header","true").load("hdfs://namenode/user/controller/ncdc-parsed-csv/20/part-r-00000")
print(df.show(10))
```

You can save this file as `demo-read.py` and run the command:

```bash
spark-submit --verbose --master yarn --deploy-mode cluster demo-read.py
```

et Viola!
