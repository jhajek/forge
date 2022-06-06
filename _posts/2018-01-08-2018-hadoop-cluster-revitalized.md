---
layout: post
status: publish
published: true
title: 'Hadoop Cluster Revitalized'
author:
  display_name: Venkatesh Naidu
  login: vchandrasekaran
  email: vchandrasekaran@hawk.iit.edu
  url: https://www.linkedin.com/in/venkatesh-naidu-chandrasekaran/
author_login: vchandrasekaran
author_email: vchandrasekaran@hawk.iit.edu
author_url: https://www.linkedin.com/in/venkatesh-naidu-chandrasekaran/
date: '2018-01-09 06:39:52 -0600'
date_gmt: '2018-01-09 12:09:52 -0600'
tags: 
- Cluster
comments: []
---

As I prepared to write this column, I have used the data from the final assignment of ITMD-521 Client/Server Technologies and Applications by professor Jeremy Hajek and worked on the assignment post semester to analyse the performance of the cluster. To benchmark the cluster, the tests or the MapReduce tasks were run on the upgraded cluster and compared with the older results. 

Though the first set of results indicated that the jobs took longer than usual to complete with the elapsed times of the tasks going beyond 5 hours and the larger tasks taking almost half a day. This is due to the fair scheduler employed in the current cluster as opposed to the fifo scheduler used last time around. Since all the tasks are run in parallel in the fair scheduler. The resources would be fairly shared among all the jobs running on the cluster. This means that the users of the cluster currently would not have to wait for the jobs previously running on the cluster to be completed before their tasks begin execution. 

The first set of results obtained after the cluster was set up shows significantly lesser time taken for the reduce, merge, shuffle and map times. Let us take a look at the following graphs to understand this better. The bar chart in green is the times taken by the optimised cluster and the time taken by the same job under the old configuration is given in red. 

![*Map Time Comparisons*](/assets/2018/01/1.png)

The above graph shows the map times of the jobs. We can see that the jobs take about the same time as the previous map times or lesser. Most of the map times are considerably lesser than the previous tasks. 

![*Reduce Times*](/assets/2018/01/2.png)

From the reduce times we can see that not a single task exceeds around 200 seconds for the reducer containers to complete the task. There is a considerable amount of time cut off in this regard which helps in the overall performance. 

![*Merge Times*](/assets/2018/01/3.png)

The merge times taken by the tasks considerably lesser for the new cluster as well. 

![*Shuffle Times*](/assets/2018/01/4.png)

These times show that the cluster is clearly optimised as there is hardly any shuffle time used by the current cluster. 

The  major changes from the previous cluster configuration is the change in the memory limits for the map and reduce tasks from 4096 to 2048 which increases the number of containers. This has an impact on the current map, reduce and shuffle times. There are additional nodes registered as well in the cluster compared to the previous version. Rack awareness is created for the current cluster. This ensures higher bandwidth and lower latency hence making it faster to get the replicas. 

We also have the graphana metrics included to monitor the cluster and its performance. 

![*Run Time Graphs*](/assets/2018/01/graphana.PNG)

It provides the metrics on the machines and forwards them to an aggregator which displays the utilization information for the jobs. The reports can be configured to display the utilization for a specific period of time or days of the week as well. 
The report displays the current utilization of CPU and memory resources, average load and the processes. The CPU utilization and memory for the entire cluster includes the resources consumed by the applications. A closer look into the CPU utilization during one of the testing phases is given below. 

![*CPU Usage*](/assets/2018/01/graphana1.PNG)

In the following post we will discuss the benchmark technique of SWIM used to test the entire cluster utilization. 
