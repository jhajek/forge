---
layout: post
title:  "Why You Need a Hadoop Cluster" 
date:   2017-07-16 20:15:25 -0500
---
## Hadoop Remote Cluster

TL; DR

Creating a shared computing cluster for Hadoop based MapReduce applciation and query design benefits the ITM department by enabling large scale research and industry best practice teaching by adjuncts and full time proffessors. Further, this resource does not exist on the IIT campus at the moment and could (properly done) open up opporturtunity to conduct joint research with other departments who are lacking this expertise and hardware capabilities.

### Abstract

The nature and need for data processing is ever increasing.  Novel methods have been designed to analyze this data.  This paper seeks to show the positive arguments for building a large scale Hadoop/MapReduce cluster that provides a department level service for researcher professors, teaching professors, and students alike.   Having this "Big Data" cluster as a resource will give our student body unparallel access to research tools as well as seperate our program in terms of tooling offered.

### History of Big Data

No history of Big Data would be replete without a short history of computing.  Computing resources and and processing power have increased steadily and predictibly due to Moore's law.  At the turn of the century we were looking at laptops and desktops with 512 megabytes of memory and server class systems with 1 or 2 gigbytes of memory.  Now we see laptop/tablet devices with 16 GB of memory and beyond and server class systems containing multiple terabes of storage and hundreds of gigabytes of memory.  

There was a force pushing the need for all this storage and processing power, and that was data creation.  Many would look to Fall of 2007 and the introduction of the iPhone and Google Android Phones shortly thereafter.  With devices in the hands of almost everyone on the planet, you can see how the wave of data was created.  Last October alone, Apple sold 65 million iPhone 7 and 20 million iPad tablets; Samsung sold a similar number of phones last year alone.  

A further thing happened, Intel x86 based hardware became a very cheap commodity.  The processors themselves became very good and the devices lasting a long time.  The laptop I am writing this on is 7 years old Core i3 with 8 GB of memory that I use as my daily driver computer for teaching and research.  It is good enough and will last a good while longer.  

At the same time, Google released two white papers, [GFS (Google File System)](https://research.google.com/archive/gfs.html) and [MapReduce](https://research.google.com/archive/mapreduce.html).  Both of these white papers, released far after Google had commercialized these technologies, became the solutions Doug Cutting was looking for.   Launching the [Hadoop project](https://hadoop.apache.org/), named after his son's toy elephant, he implemented the Google Filesystem into the HDFS (Hadoop Filesystem).  This filesystem is optimized for handling large scale data and for processing large scale ad-hoc queries.  In fact much in the same way a drag racer car is optimized to win a short quarter mile race, not to drive to the store for milk, in the same way HDFS is optimized.  

The MapReduce paper by Google was written to explain the ways and theories behind Google's handling, indexing, and qerying large amounts of data.  In 2005, when the paper was released we would assume they had a scale of data problem that no one else on the planet had relating to indexing the web.  [Doug Cutting](https://en.wikipedia.org/wiki/Doug_Cutting) implemented the MapReduce paradigm in Java, combined with the HDFS, and created and opensourced the Hadoop platform.


### Big Data's arrival

The creation of and nuturing of the Hadoop project initially started by Doug Cutting and a few like minded people in early 2005, this was a project initially called Nutch and set out to reverse engineer the Google search engine platform in order to provide and opensource alternative.   The project had many sub-projects, a webcrawler, and indexer, and a search algorithm.   Initially this work was stored and indexed on a SQL database.  Doug Cutting realized that the amount of storage and the nature of searching was poorly aligned to using RDBMS on top of a traditional (UFS, ext3, ext4) based filesystem and at the time, MySQL.  These filesystems were not designed to handle this kind of work.

### A Perfect Storm

MapReduce works in a way the is different from SQL and RDBMS in general--[Michael Stonebreaker](https://en.wikipedia.org/wiki/Michael_Stonebraker "Michael Stonebraker") to this day is highly highly critical of Hadoop and MapReduce.  I prefer to look at it as different tools for different problems and MapReduce and SQL and not fair comparisons.  In 2007, Doug Cutting was hired by Yahoo to implement Hadoop accross Yahoo for its web search indexing, replacing an outdated in house solution.  

#### Short Primer: How is MapReduce different from SQL 

MapReduce in the simplest sense works by breaking a data query up. 

MapReduce is functional programming language, SQL is declarative by design. SQL starts stating the result you want and lets the database engine figure out how to get it.  MapReduce allows you to specify how the actual steps of data processing happen MapReduce is written in Java.  This difference is the key to processing large numbers of gigabytes up to petabytes.  
  
The Hadoop system capitalizes on 4 distinct technology advantages that had become accessable and prevelant in late 2008-2010 that were not in existance during the development of the early RDMBS:

- Accessiblity
    + Hadoop is designed from the ground up to run on commodity x86 based hardware
- Robust
    + Hadoop is architected to assume frequent hardware failures (Reliability)
- Scalable  
    + Hadoop scales linearly by simply adding commodity nodes (Servers)
- Simple
    + Hadoop code (MapReduce) is automatically parallelized accross a cluster

### Scaling a Cluster

Seeing as a cluster can scale lineraly by adding commodity hardware this becomes and advantage as any 86 based system (real or virtual) can be added to increase capacity.  With the nature of parallel jobs being run, this allows for maximum hardware usage and efficency.

#### The Need for a Cluster

In (spring of 2018) being the fifth time I have taught from [Hadoop the Definitive Guide 4th edition](http://shop.oreilly.com/product/0636920033448.do) I have gained a deeper appreciation.  With the increase of laptop hardware I have seen great strides been made in the difference of what can be taught/demonstrated in a class compared to 10 years ago.  With the great advances, data has advanced too.  With there being hardware available, the installation, configuration, and management of a Hadoop Cluster is now with in the reache of a single person and or small IT team.  

The Hadoop Nodes for our current course consist of:

* Namenode  - 1.8 TB of storage, 32 GB of RAM, [AMD FX 6100 Hexacore](https://en.wikipedia.org/wiki/List_of_AMD_FX_microprocessors#Bulldozer_Core_.28Zambezi.2C_32_nm.29)
* Datanode1 - 3.6 TB of storage, 96 GB of RAM, 2x [Intel Xeon E5530 Quad Core](https://ark.intel.com/products/37103/Intel-Xeon-Processor-E5530-8M-Cache-2_40-GHz-5_86-GTs-Intel-QPI)
* Datanode2 - 2 TB of storage, 32 GB of RAM, 2x [Intel Xeon E5530 Quad Core](https://ark.intel.com/products/37103/Intel-Xeon-Processor-E5530-8M-Cache-2_40-GHz-5_86-GTs-Intel-QPI)
* Datanode3 - 2 TB of storage, 32 GB of RAM, 2x [Intel Xeon E5504 Quad Core](https://ark.intel.com/products/40711/Intel-Xeon-Processor-E5504-4M-Cache-2_00-GHz-4_80-GTs-Intel-QPI)
* Datanode4 - 2 TB of storage, 24 GB of RAM, [AMD FX 6100 Hexacore](https://en.wikipedia.org/wiki/List_of_AMD_FX_microprocessors#Bulldozer_Core_.28Zambezi.2C_32_nm.29)
 
You may notice that these chips and this memory is a few years old and is not homogenous.  This might be a problem in a production system, but these systems will do quite fine.  On top of these systems, using Cobbler to pre-configure the install severely reduces install time and gives us a chance to quickly reinstall if there is ever a problem. 

Now students have access initially to multiple hundreds of Gigabyte datasets, and those hundreds of Gigabyts will grow larger until we have students working with terabytes of data.  There are a prolifferation of datasets available.  Most major cities have data portals, the government has census data, NASA has space data, and so forth.  There are datasets that can be purchased as well.  There is no reason not to be teaching about this technology.

#### What to teach with a Cluster?

My own background is in operations and orchestration.  One way to look at *Big Data* is optimizations--a small correction or optimization will playout big over a large set of data.  My specific area is not in Analytics or algorithms, but I can provide a sampling and the tooling foundation to arm prospective students with the correct infromation when it come time to make decisions.  

