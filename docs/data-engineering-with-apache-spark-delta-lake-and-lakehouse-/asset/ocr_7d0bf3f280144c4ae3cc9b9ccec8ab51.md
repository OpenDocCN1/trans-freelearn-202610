In short, you are paying for something that you are not using... and no one likes to do that.

In many respects, moving toward cloud computing has been a game-changer. Having a data lake in the cloud allows us to achieve something that was not possible before: we can finally decouple storage and compute!

The following diagram shows a loosely coupled data lake:

![](bbox=[0.22, 0.27, 0.77, 0.62])

Figure 2.9 – A loosely coupled data lake

Decoupling storage from compute offers several advantages, outlined as follows:

With a loosely coupled data lake, the cheap (storage) layer is permanent, whereas the expensive (compute) layer is temporary. How does it work this way?

Typically, during batch processing, computations are performed only for a brief period each day (usually just a few hours). Having your infrastructure on the cloud allows you to create a distributed processing cluster (Hadoop/Spark/Flink) every day, just before batch processing starts; all the jobs are then run and, finally, the cluster is deleted right after all the jobs finish. Since the storage portion exists outside the compute cluster, there is no data loss if the cluster is destroyed. This process is depicted in the following diagram: