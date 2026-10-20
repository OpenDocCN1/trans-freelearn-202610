We can enable Delta caching by selecting one of the **Delta Cache Accelerated** workers from the cluster configuration, as illustrated in the following screenshot:

![](bbox=[0.14, 0.15, 0.86, 0.73])

Figure 5.8 – Selecting a Delta Cache accelerated storage

Then, we continue as follows:

+   - To enable the Spark cache, we need to run the cache option on our dataframe—as shown in the following Python command—where we cache a dataframe called data:

```
data.cache()
```