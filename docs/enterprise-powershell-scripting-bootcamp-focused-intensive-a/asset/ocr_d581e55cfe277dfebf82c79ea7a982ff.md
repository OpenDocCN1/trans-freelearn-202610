# Creating a windows event log source

In the instance that you want to write to the Windows event log, one of the prerequisites is to register the script as an event source with the Windows event log system. This is done with the use of the New-EventLog cmdlet. You start by specifying the New-EventLog cmdlet, followed by the -LogName parameter followed by an event log name. You can either select your own event log or specify one of the built-in event logs, such as Application or system. Finally, you specify the -Source parameter with the name of the script you are executing.

You can only register the event log source once, which may lead to errors when executing a script multiple times on the same system. You can retrieve the event log sources by specifying the get-eventlog cmdlet with the -LogName parameter, and piping the results to Select-object Source -Unique. This is not very desirable as it requires the system to query all the log events to determine if a source is already registered. Since Windows systems have a large number of event logs and log events, the query is very CPU and memory intensive.

The fastest way to get around a duplicate event log source error message is to suppress the error message itself. This is done through leveraging the -ErrorAction parameter set to SilentlyContinue with the New-EventLog cmdlet. The -ErrorAction parameter with SilentlyContinue specified will capture the error and silently continue with the script, suppressing the error messages.

To register a new event log source suppressing error messages, you can perform the following:

```
New-EventLog -LogName Application -Source "WindowsServerScanningScript" -ErrorAction SilentlyContinue
```

The output of the PowerShell window will look like the following:

![](bbox=[0.08, 0.78, 0.92, 0.81])

The preceding script displays how to register a new event log source with the Windows event log system. You first start by using the New-EventLog cmdlet with the -LogName parameter set to Application. You then specify the -Source argument and set it to the script name of "WindowsServerScanningScript". Finally, you suppress error messages by using the -ErrorAction argument and setting it to SilentlyContinue. After executing this command, you will be able to write to the Application event log with the event source of WindowsServerScanningScript.