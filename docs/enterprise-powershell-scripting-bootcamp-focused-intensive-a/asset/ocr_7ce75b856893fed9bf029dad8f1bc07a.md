# Scheduled tasks

Scheduled tasks enable you to schedule automated actions on a system. Microsoft has a wide variety of built-in scheduled tasks, which include time synchronization, universal plug and play, Windows updates, and disk defragmentation. These task actions are invoked by different trigger criteria and automatically run on your system.

You can retrieve the list of scheduled tasks on a system by using the get-scheduledtask cmdlet. The get-scheduledtask cmdlet reveals a list of scheduled task names, their taskpath, and their current state. You may also return other properties such as author, date, description, version, and documentation.

To retrieve and count scheduled tasks on a system, you can leverage the following:

```
get-scheduledtask
(get-scheduledtask).count
```

The output of this command would look like this:

![](bbox=[0.17, 0.41, 0.88, 0.92])