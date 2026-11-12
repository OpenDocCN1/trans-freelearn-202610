> I would recommend Queueables over @future since they have additional flexibility in terms of the type of information that can be passed to them and they are more easily tracked once submitted.

# Queueables

Consider the following implementation guidelines when utilizing Queueables:

+   - Avoid flooding the async queue with many jobs (consider Batch Apex).
+   - Methods must execute quickly or risk platform throttling, which will delay the execution.
+   - Implement the interface `Queueables` and the execute method. Then, use the `System.enqueueJob` method to start your job:
+     `public void execute(QueueableContext context)`
+   - The object implementing `Queueables` can contain serializable state that represents the work to be undertaken. This can be complex lists, maps, or your own Apex types.
+   - Typically, the time taken to dequeue and execute the work is less than Batch Apex.
+   - Be careful while processing lists, ensuring that you volume test with large volumes of lists to ensure that the job can complete with maximum data.
+   - You can track execution as the **Apex Job ID** is returned.
+   - Executing from an Apex Trigger should be considered carefully. Use of `Queueables` should be bulkified, and if there is a possibility of bulk data loading, there is a greater chance of hitting the daily per user limit for `@future jobs`.
+   - You can start up to 50 `queueables` jobs in a sync context.
+   - You can start only one `queueable` job within an existing `queueable` job; this is known as chaining. The depth of the chain is unlimited, though the platform will slow the gaps between jobs as the depth grows. Also Developer Edition orgs are limited to a depth of 5.
+   - If code attempts to issue more than 1 `queueable` job within an existing `queueable` context, an error occurs. This can be particularly an issue if your packaged code can be invoked by code (such as that written by developer X) in a subscriber org. If they have their own async methods indirectly invoking your use of this feature, an exception will occur. As such, try to avoid using these features from a packaged Apex Trigger context.