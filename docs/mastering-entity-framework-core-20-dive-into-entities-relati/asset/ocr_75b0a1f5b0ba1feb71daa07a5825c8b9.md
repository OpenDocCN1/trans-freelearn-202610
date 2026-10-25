We have already seen the concurrency conflicts while trying to edit posts in different tabs, which is kind of a simulation of a real-time concurrency conflict. Since we have handled the concurrency, let's now see how it stops the user from persisting values into the data store.

The post highlighted in the following screenshot will have a Url value of **test-1**, and if the value is changed, then any other new persistence will be rejected by the framework:

![](bbox=[0.104, 0.243, 0.895, 0.638])