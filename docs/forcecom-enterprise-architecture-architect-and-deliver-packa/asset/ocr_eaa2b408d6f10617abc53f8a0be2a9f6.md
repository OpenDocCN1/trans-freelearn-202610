Asynchronous Processing in Force.com: https://developer.salesforce.com/page/Asynchronous_Processing_in_Force_com.

> Apex Scheduler can be used to execute code in an async context via its execute method. This also provides enhanced governors. Depending on the processing involved, you might choose to start a Batch Apex job from the execute method. Otherwise, if you want a one-off, scheduled execution, you can use the Database.scheduleBatch method.

# @future

Consider the following implementation guidelines when utilizing @future jobs:

+   * Avoid flooding the async queue with many @future jobs (consider Batch Apex).
+   * Methods must execute quickly or risk platform throttling, which will delay the execution.
+   * It is easier to implement as they require only a method annotation to be added.
+   * Typically, the time taken to dequeue and execute the work is less than Batch Apex.
+   * You can express, via a method annotation, an increase in a specific governor to customize the governor limits to your needs.
+   * Parameter types are limited to simple types and lists.
+   * Be careful when passing in list parameters by ensuring that you volume test with large lists.
+   * There is no way to track execution as the Apex Job ID is not returned.
+   * Executing from an Apex Trigger should be considered carefully; @future methods should be bulkified, and if there is a possibility of bulk data loading, there is a greater chance of hitting the daily per user limit for @future jobs.
+   * You cannot start another @future, Batch Apex or Queueables job. This is particularly an issue if your packaged code can be invoked by code (such as that written by developer X) in a subscriber org. If they have their own @future methods indirectly invoking your use of this feature, an exception will occur. As such, try to avoid using these features from a packaged Apex Trigger context.