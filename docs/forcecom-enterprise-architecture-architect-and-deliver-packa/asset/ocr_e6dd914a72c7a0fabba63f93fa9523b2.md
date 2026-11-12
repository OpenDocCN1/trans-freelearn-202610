+   - It is possible to run more queueables jobs concurrently than Batch Apex.
+   The exact limit is not documented. However be sure to avoid flooding the
+   system, by putting in place logic that controls how chaining and concurrency
+   are used.

# Batch Apex

In our scenario, the **Race Data** records loaded into Salesforce do not by default
associate themselves with the applicable **Contestant** records. As noted earlier, the
**Contestant** record has a **Race Data Id** field that contains an Apex-calculated unique
value made up from the **Year**, **Race Name**, and **Driver Id** fields concatenated
together.

The following Batch Apex job is designed to process all the **Race Data** records,
calculate the compound **Contestant Race Data Id** field, and update the records with
the associated **Contestant** record relationship. The following example highlights
a number of other things in terms of the Selector and Service layer patterns usage
within this context:

+   - The `RaceDataSelector` class is used to provide the `QueryLocator` method
+   needed, keeping the SOQL logic (albeit simple in this case) encapsulated in
+   the Selector layer.

+   - The `RaceService.processData` method (added to support this job) is
+   passed in the IDs of the records rather than the records passed to the
+   execute method in the `scope` parameter. This general approach avoids any
+   concern over the record data being old, as Batch Apex serialized all record
+   data at the start of the job.

+   - As per the standard practice, the Service layer does not handle exceptions;
+   these are thrown to the caller and it handles them appropriately. In this case,
+   a new `LogService` class is used to capture exceptions thrown and log them
+   to a Custom Object, utilizing the Apex job ID as a means to differentiate log
+   entries from others jobs. Note that the use of `LogService` is for illustration
+   only; it is not implemented in the sample code for this chapter.

+   - In the `finish` method, new `NotificationService` encapsulates logic,
+   which is able to notify end users (perhaps via e-mail, tasks, or Chatter as
+   considered earlier) of the result of the job, passing on the Apex job ID, such
+   that it can leverage the information stored in the log Custom Object and/or
+   `ApexAsyncJob` to form an appropriate message to the user. Again, this is for
+   illustration only.