# Troubleshooting

The first line prints out the daemon type and version. Depending on how it was compiled, it might also include the current SVN revision number. A list of the compiled-in features follows—very useful to know whether you should expect SNMP, IPMI, or VMware monitoring to work at all. Then, the path to the currently used configuration file is shown—helpful when we want to figure out whether the file we changed was the correct one. In the server and proxy log files, both the current and the required database versions are present—we discussed those in *Chapter 22, Zabbix Maintenance.*

After the database versions, the internal process startup messages can be found:

```
2583:20151231:155712.323 server #0 started [main process]
2592:20151231:155712.334 server #5 started [poller #3]
2591:20151231:155712.336 server #4 started [poller #2]
2590:20151231:155712.337 server #3 started [poller #1]
2593:20151231:155712.339 server #6 started [poller #4]
```

There will be many more lines like these; the output here is trimmed. This might help verify that the expected number of processes of some type has been started. When looking at log file contents, it is not always obvious which process logged a specific line—and this is where the startup messages can help. If we see a line such as the following, we can find out which process logged it:

```
21974:20151231:184520.117 Zabbix agent item "vfs.fs.size[/,free]" on
host "A test host" failed: another network error, wait for 15 seconds
```

We can do that by looking for the startup message with the same PID:

```
# grep 21974 zabbix_server.log | grep started

21974:20151231:184352.921 server #8 started [unreachable poller #1]
```

![](bbox=[0.135, 0.641, 0.865, 0.693])

> If more than one line is returned, apply common sense to find out the startup message.

This demonstrates that hosts are deferred to the unreachable poller after the first network failure.

But what if the log file has been rotated and the original startup messages are lost? Besides more advanced detective work, there's a simple method, provided that the daemon is still running. We will look at that method a bit later in this chapter.