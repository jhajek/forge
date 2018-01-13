Cluster Revitalized. 
As I prepared to write this column, I have used the data from the final assignment of ITMD-521 Client/Server Technologies and Applications by professor Jeremy Hajek and worked on the assignment post semester to analyse the performance of the cluster. To benchmark the cluster, the tests or the MapReduce tasks were run on the upgraded cluster and compared with the older results. 

Though the first set of results indicated that the jobs took longer than usual to complete with the elapsed times of the tasks going beyond 5 hours and the larger tasks taking almost half a day. This is due to the fair scheduler employed in the current cluster as opposed to the fifo scheduler used last time around. Since all the tasks are run in parallel in the fair scheduler. The resources would be fairly shared among all the jobs running on the cluster. This means that the users of the cluster currently would not have to wait for the jobs previously running on the cluster to be completed before their tasks begin execution. 

The first set of results obtained after the cluster was set up shows significantly lesser time taken for the reduce, merge, shuffle and map times. Let us take a look at the following graphs to understand this better. The bar chart in green is the times taken by the optimised cluster and the time taken by the same job under the old configuration is given in red. 

![img1](https://github.com/illinoistech-itm/vchandrasekaran/blob/master/ITMD-521/Images/1.png)

The above graph shows the map times of the jobs. We can see that the jobs take about the same time as the previous map times or lesser. Most of the map times are considerably lesser than the previous tasks. 

![img2](https://github.com/illinoistech-itm/vchandrasekaran/blob/master/ITMD-521/Images/2.png)

From the reduce times we can see that not a single task exceeds around 200 seconds for the reducer containers to complete the task. There is a considerable amount of time cut off in this regard which helps in the overall performance. 

![img3](https://github.com/illinoistech-itm/vchandrasekaran/blob/master/ITMD-521/Images/3.png)

The merge times taken by the tasks considerably lesser for the new cluster as well. 

![img4](https://github.com/illinoistech-itm/vchandrasekaran/blob/master/ITMD-521/Images/4.png)

These times show that the cluster is clearly optimised as there is hardly any shuffle time used by the current cluster. 

The  major changes from the previous cluster configuration is the change in the memory limits for the map and reduce tasks from 4096 to 2048 which increases the number of containers. This has an impact on the current map, reduce and shuffle times. There are additional nodes registered as well in the cluster compared to the previous version. Rack awareness is created for the current cluster. This ensures higher bandwidth and lower latency hence making it faster to get the replicas. 

#Graph with graphana metrics after running all the tasks with full cpu utilization #
#Description of graphana metrics#

In the following post we will discuss the benchmark technique of SWIM used to test the entire cluster utilization. 
