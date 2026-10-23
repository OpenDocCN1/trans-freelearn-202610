This is because the Windows event log system stores the event in binary (proprietary) format. Now, if we view that same event log in Windows' native **Event Viewer**, things are a lot clearer, as we can see in the following screenshot:

![](bbox=[0.14, 0.16, 0.86, 0.58])

Figure 5.4 – Windows' Event Viewer

You will find that a large part of the challenge in collecting event logs is around parsing and interpreting a large variety of event logs in a way that is scalable, reliable, and allows correlation of that variety of logs. Again, a SIEM can help with this task, and we will actually look at implementing parsing of logs in *Chapter 9, Visualizing, Correlating, and Alerting*.

Event logs will play a major role in the security monitoring efforts of your ICS environment. Make sure you take the time to properly configure your systems, devices, and equipment to send interesting events to a local repository (SIEM). In *Chapter 9, Visualizing, Correlating, and Alerting*, we will see how to do this for Windows systems and look at which events to send over. As for the many network, automation, and controls system and equipment logs, most of those will likely have an option to send logs to an external syslog entity. Take some time to configure the equipment to send (forward) their event logs to a SIEM server. Syslog works with log severity levels. These are the severity levels of a typical syslog implementation: