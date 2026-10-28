
And what if you set up Zabbix frontend monitoring, like we did in Chapter 13, *Monitoring Web Pages*? You are likely to see only such records, as our web scenario logs in and out every minute. But notice the filter—we may also filter by user, action, and resource:

![](img/f5313a804532fabacc8d07cc8607eeba_691_0.png)

Expand the **Action** and **Resource** dropdowns—notice that they are quite fine grained, especially the **Resource** dropdown.

In the Zabbix 1.8 version of this book, it said:

> > In the first Zabbix 1.8 releases some actions are not registered in the audit log.  
> > Such issues are expected to be fixed in near future.

Oh well. Unfortunately, it did not get fixed in further 1.8 releases, 2.0, 2.2, or 2.4. Nor in 3.0. The Zabbix audit log is still missing lots of operations performed, especially when the API is used. While the audit log can be extremely useful, it can easily miss the specific operation you are interested in. Perform a test with the version you are interested in to be sure—the list of operations that are not logged can easily change in a minor version.

Moving forward from the sad fact of the broken audit log, as an exercise, try to find out at what time you added the **Restart Apache** action.

While looking at this section, let's remind ourselves of another logging area—the action log that we briefly looked at before. Go to **Reports | Action log**. Here, all actions performed by the Zabbix server are recorded. This includes sending e-mails, executing remote commands, sending SMS messages, and executing custom scripts. This view provides information on what content was sent to whom, whether it was successful, and error messages, if any. It is useful for verifying whether Zabbix has or has not sent a particular message as well as figuring out whether the configured actions are working as expected.

Together, the action and log audit sections provide a good overview of internal Zabbix configuration changes as well as debugging help to determine what action operations have been performed.

## Exploring configuration file parameters

Let's conclude this chapter by digging into the configuration files of the Zabbix agent and server and examining each parameter in them. We'll start with the agent configuration file and discuss the ways in which common parameters apply to other daemons. We will skip the proxy configuration file, as the common parameters will be discussed by then, and the proxy-specific parameters were discussed in Chapter 19, Using Proxies to Monitor Remote Locations. We will also skip all the parameters that start with TLS, as those are related to Zabbix daemon traffic encryption, and we discussed that in Chapter 20, Encrypting Daemon Traffic.

We will look at the parameters in the order they appear in in the default example configuration files — no other meaning should be derived from the ordering here.

While reading the following descriptions, it is suggested to have the corresponding configuration file open. It will allow you to verify that the parameters are the same in your version of Zabbix. Make sure to read the comments next to each parameter — they might show that some parameters have changed since the time of writing this. In general, when in doubt, read the comments in the configuration files. The Zabbix team tries really hard to make them both short and maximally relevant and helpful.

### Zabbix agent daemon and common parameters

Let's start with the agent daemon parameters. For the parameters that are also available for other daemons, we'll discuss their relevance to all the daemons here:

- PidFile: This is common to all daemons. They write the PID of the main process in this file. The default configuration files use /tmp for simplicity's sake. In production systems, this should be set to the distribution recommended location.

- LogType: This is common to all daemons and can be one of file, syslog, or console. The default is file, and in that case, the LogFile parameter determines where the logs are written. The syslog value directs the daemon to log to syslog, and the console parameter tells it to log the messages to stdout.

- LogFile: This is common to all daemons. Log data is written to this file when LogType is set to file. The default configuration files use /tmp for simplicity's sake. In production systems, this should be set to the distribution-recommended location.

- LogFileSize: This is common to all daemons. When logging to a file, if the file size exceeds this number of megabytes, move it to file.0 (for example, zabbix_agentd.log.0) and log to a new file. Only one such move is performed (that is, there is never zabbix_agentd.log.1).

- DebugLevel: This is common to all daemons and specifies how much logging information to provide, starting with 0 (nearly nothing) and ending with 5 (a lot). It is probably best to run at DebugLevel 3 normally, and use something higher for debugging. For example, starting with DebugLevel 4, all server and proxy database queries are logged. At DebugLevel 5, two extra things are currently logged:
  - Received pages for web monitoring
  - Received raw data for VMware monitoring

- SourceIP: This is common to all daemons. If the system has multiple interfaces, outgoing connections will use the specified address. Note that not all connections will obey this parameter—for example, the backend database connections on the server or proxy won't.

- EnableRemoteCommands: This determines whether the system.run item should allow running commands. Disabled by default.

- LogRemoteCommands: If EnableRemoteCommands is enabled, this parameter allows us to log all the received commands. Unless system.run is used to retrieve data, it's probably a good idea to enable logging of the remote commands.

- Server: This is also available for the Zabbix proxy, but not for the Zabbix server. It's a comma-delimited list of IP addresses or host names the agent should accept connections from. It's only relevant for passive items, zabbix_get, and other incoming connections.

- ListenPort: This is common to all daemons and specifies the port to listen on.

- ListenIP: This is common to all daemons and specifies the IP address to listen on—could also be a comma-delimited list of addresses.

> We will look at changing the log level for a running daemon in *Appendix A, Troubleshooting.*

### Zabbix Maintenance

- StartAgents: This is the number of processes to start that are responsible for incoming connection handling. If it's a very resource-starved system, it might be a good idea to reduce this. If this agent is expected to get lots of queries for passive items, increase this number. Note that it has nothing to do with the collector or active check processes; their numbers cannot be directly changed. If set to 0, the agent will stop listening to incoming connections. This could be better security-wise, but could also make debugging much harder.

- ServerActive: This is the list of servers and ports to connect to for active checks. It follows the syntax of server:port, with multiple entries delimited by comma. If not set, no active checks are processed. We discussed this functionality in *Chapter 3, Monitoring with Zabbix Agents and Basic Protocols*.

- Hostname: This is also available for the Zabbix proxy, but not for the Zabbix server. If specified, the exact string will be sent to the Zabbix server as the host name for this system.

- HostnameItem: If Hostname is not specified but HostnameItem is, the value in this parameter will be interpreted as an item key and the result of the evaluation will be sent to the server as the host name for this system.

- HostMetadata: This is an exact string to be sent to the server—used in active agent autoregistration.

- HostMetadataItem: If HostMetadata is not specified but HostMetadataItem is, the value in this parameter will be interpreted as an item key and the result of the evaluation will be sent to the server as the host metadata to be used in active agent autoregistration.

- RefreshActiveChecks: This specifies how often the agent should connect to the server and ask for active items. It's set to 2 minutes by default. If active checks are not used at all, it means a useless connection every 2 minutes from each agent—it's best not to set ServerActive at all in such a case.

- BufferSend: Active agents will send values every BufferSend seconds—by default, every 5 seconds. This allows us to reduce the number of network connections if multiple values are collected within a 5-second window.

- BufferSize: This is a buffer to hold the values for active items. By default, it's set to 100 values. This is an in-memory buffer—do not set it too large if memory usage is a concern. The buffer is actually split in half if there is at least one log-monitoring item—one half is used for "normal" values, the other for log entries. If the buffer is full, new "normal" values will result in the dropping of older "normal" values, but it won't affect log entries. If the log entry half of the buffer is full, log file processing stops, but no entries are dropped there. If there are log items only and no "normal" items, half of the buffer is still reserved for "normal" entries. If there are only "normal" items, the whole buffer is used for them until at least one log item is added.
