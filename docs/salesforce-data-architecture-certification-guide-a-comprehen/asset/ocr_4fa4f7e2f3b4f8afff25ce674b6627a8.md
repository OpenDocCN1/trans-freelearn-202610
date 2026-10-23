```
```apex
update accounts;
}

public void finish(Database.BatchableContext info) {
    // Do nothing
}
}
```
```

This class will be invoked as follows:

```
```apex
Id batchJobId = Database.executeBatch(new
appendAccountNameBatch());
```
```

Batch Apex jobs can be viewed (and stopped) in Salesforce Setup by typing Apex Jobs into the quick-find box. If a job is **Completed**, then it means it ran without errors. Jobs that have a status of **Aborted** or **Failed** require investigation because they have not been executed successfully.

It’s entirely possible to schedule batch Apex jobs to run according to a schedule, say every night. To do that, you need to create a wrapper class for your batch Apex class that implements the Schedulable interface, and then schedule that class to be executed according to a schedule, defined by using the Unix-like cron format.

> **Important note**
> 
> The **Unix** cron format is used to specify a frequency for which schedulable jobs are to be run. It has origins in the cron utility on Unix operating systems. It follows a format of six values for day/time (with an optional seventh for the year). It is represented as a string, such as `0 0 17 * * ?`, for a daily execution at 5 P.M. More information on the format of the cron string can be found at `https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_scheduler.htm`.

For example, let’s wrap our appendAccountNameBatch class to allow it to be scheduled:

```
```apex
public class scheduledAppendAccountBatch implements
Schedulable {
    public void execute(SchedulableContext sc) {
    appendAccountNameBatch b = new appendAccountNameBatch (); //
your batchclas
    database.executebatch(b);
    }
}
```
```