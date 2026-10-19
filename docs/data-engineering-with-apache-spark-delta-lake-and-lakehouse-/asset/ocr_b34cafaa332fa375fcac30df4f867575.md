You can see a diagram of the process here:

![](bbox=[0.105, 0.125, 0.845, 0.545])

Figure 2.19 – State of aggregate view at time t

At time t + 1, the stream-processing code recomputes data from 2018 up to the most recent event within the sales topic. Notice that new events have been added to the queue since the last time the computations were performed. The recomputed data is published as follows:

+   - The previous version of the AGGREGATED SALES view is dropped.
+   - A new version of the AGGREGATED SALES view is computed to take its place.
+   - Queries are pointed to the recomputed view.