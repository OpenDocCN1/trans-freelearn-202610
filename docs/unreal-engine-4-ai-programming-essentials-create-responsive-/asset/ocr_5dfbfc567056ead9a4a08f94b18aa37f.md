Selector executes each of its children from left to right and doesn't fail; however, it returns success when one of its children returns success. So, this is good for a state that doesn't check for successfully executed nodes. The following screenshot shows an example of Selector:

![](bbox=[0.301, 0.221, 0.693, 0.345])

Sequence executes its children in a similar fashion to Selector but returns fail when one of its children returns fail. This means that it's required that all nodes return success to complete the sequence. You can see a Sequence node in the following screenshot:

![](bbox=[0.301, 0.426, 0.693, 0.539])

Last but not least, Simple Parallel allows you to execute a task and a tree essentially at the same time. This is great for creating a state that requires another task to always be called. To set it up, you need to first connect it to a task that it will execute. The second task or state connected continues to be called with the first task until the first task returns success.