
Asynchronous Processing in Force.com: https://developer.salesforce.com/page/Asynchronous_Processing_in_Force_com.

> Apex Scheduler can be used to execute code in an async context via its execute method. This also provides enhanced governors. Depending on the processing involved, you might choose to start a Batch Apex job from the execute method. Otherwise, if you want a one-off, scheduled execution, you can use the Database.scheduleBatch method.

## @future

Consider the following implementation guidelines when utilizing @future jobs:

- Avoid flooding the async queue with many @future jobs (consider Batch Apex).
- Methods must execute quickly or risk platform throttling, which will delay the execution.
- It is easier to implement as they require only a method annotation to be added.
- Typically, the time taken to dequeue and execute the work is less than Batch Apex.
- You can express, via a method annotation, an increase in a specific governor to customize the governor limits to your needs.
- Parameter types are limited to simple types and lists.
- Be careful when passing in list parameters by ensuring that you volume test with large lists.
- There is no way to track execution as the Apex Job ID is not returned.
- Executing from an Apex Trigger should be considered carefully; @future methods should be bulkified, and if there is a possibility of bulk data loading, there is a greater chance of hitting the daily per user limit for @future jobs.
- You cannot start another @future, Batch Apex or Queueables job. This is particularly an issue if your packaged code can be invoked by code (such as that written by developer X) in a subscriber org. If they have their own @future methods indirectly invoking your use of this feature, an exception will occur. As such, try to avoid using these features from a packaged Apex Trigger context.

> I would recommend Queueables over @future since they have additional flexibility in terms of the type of information that can be passed to them and they are more easily tracked once submitted.

## Queueables

Consider the following implementation guidelines when utilizing Queueables:

- Avoid flooding the async queue with many jobs (consider Batch Apex).
- Methods must execute quickly or risk platform throttling, which will delay the execution.
- Implement the interface `Queueable` and the execute method. Then, use the `System.enqueueJob` method to start your job:
  `public void execute(QueueableContext context)`
- The object implementing `Queueable` can contain serializable state that represents the work to be undertaken. This can be complex lists, maps, or your own Apex types.
- Typically, the time taken to dequeue and execute the work is less than Batch Apex.
- Be careful while processing lists, ensuring that you volume test with large volumes of lists to ensure that the job can complete with maximum data.
- You can track execution as the **Apex Job ID** is returned.
- Executing from an Apex Trigger should be considered carefully. Use of `Queueables` should be bulkified, and if there is a possibility of bulk data loading, there is a greater chance of hitting the daily per user limit for @future jobs.
- You can start up to 50 `queueables` jobs in a sync context.
- You can start only one `queueable` job within an existing `queueable` job; this is known as chaining. The depth of the chain is unlimited, though the platform will slow the gaps between jobs as the depth grows. Also Developer Edition orgs are limited to a depth of 5.
- If code attempts to issue more than 1 `queueable` job within an existing `queueable` context, an error occurs. This can be particularly an issue if your packaged code can be invoked by code (such as that written by developer X) in a subscriber org. If they have their own async methods indirectly invoking your use of this feature, an exception will occur. As such, try to avoid using these features from a packaged Apex Trigger context.
- It is possible to run more queueables jobs concurrently than Batch Apex. The exact limit is not documented. However be sure to avoid flooding the system, by putting in place logic that controls how chaining and concurrency are used.

## Batch Apex

In our scenario, the **Race Data** records loaded into Salesforce do not by default associate themselves with the applicable **Contestant** records. As noted earlier, the **Contestant** record has a **Race Data Id** field that contains an Apex-calculated unique value made up from the **Year**, **Race Name**, and **Driver Id** fields concatenated together.

The following Batch Apex job is designed to process all the **Race Data** records, calculate the compound **Contestant Race Data Id** field, and update the records with the associated **Contestant** record relationship. The following example highlights a number of other things in terms of the Selector and Service layer patterns usage within this context:

- The `RaceDataSelector` class is used to provide the `QueryLocator` method needed, keeping the SOQL logic (albeit simple in this case) encapsulated in the Selector layer.

- The `RaceService.processData` method (added to support this job) is passed in the IDs of the records rather than the records passed to the execute method in the `scope` parameter. This general approach avoids any concern over the record data being old, as Batch Apex serialized all record data at the start of the job.

- As per the standard practice, the Service layer does not handle exceptions; these are thrown to the caller and it handles them appropriately. In this case, a new `LogService` class is used to capture exceptions thrown and log them to a Custom Object, utilizing the Apex job ID as a means to differentiate log entries from others jobs. Note that the use of `LogService` is for illustration only; it is not implemented in the sample code for this chapter.

- In the `finish` method, new `NotificationService` encapsulates logic, which is able to notify end users (perhaps via e-mail, tasks, or Chatter as considered earlier) of the result of the job, passing on the Apex job ID, such that it can leverage the information stored in the log Custom Object and/or `ApexAsyncJob` to form an appropriate message to the user. Again, this is for illustration only.

The following code shows the implementation of the Batch Apex job to process the race data records:

```java
public class ProcessRaceDataJob
implements Database.Batchable<SObject> {
public Database.QueryLocator start(
Database.BatchableContext ctx) {
return
RaceDataSelector.newInstance().selectAllQueryLocator();
}

public void execute(
Database.BatchableContext ctx, List<RaceData__c> scope) {
try {
Set<Id> raceDataIds =
new Map<Id, SObject>(scope).keySet();
RaceService.processData(raceDataIds);
}
catch (Exception e) {
LogService.log(ctx.getJobId(), e);
}
}

public void finish(Database.BatchableContext ctx) {
NotificationService.notify(
ctx.getJobId(), 'Process Race Data');
}
}
```

Note that even the code to start the Apex job is implemented in a Service method; this allows for certain pre-checks for concurrency and configuration to be encapsulated. To run this job, a list view Custom Button has been placed on the Race Data object in order to start the process. The controller calls the Service method to start the job as follows:

```
Id jobId = RaceService.runProcessDataJob();
```

Consider the following implementation guidelines when utilizing Batch Apex:

- A maximum of 5 active Apex Jobs can run in the org.
- **Apex Flex Queue** (available under **Setup**) can queue up to 100 jobs. The platform automatically removes items from this queue when jobs complete. You can also reorder items in the queue through the UI or via an API. Jobs in the queue have a special holding status.
