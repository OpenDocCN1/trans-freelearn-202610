
# Monitoring a single file

Let's start with the simpler case, monitoring a single file. To do so, we could create a couple of test files. To keep things a bit organized, let's create a directory /tmp/zabbix_logmon/ on A test host and create two files in there, logfile1 and logfile2. For both files, use the same content as this:

```
2016-08-13 13:01:03 a log entry
2016-08-13 13:02:04 second log entry
2016-08-13 13:03:05 third log entry
```

> Active items must be properly configured for log monitoring to work—we did that in Chapter 3, Monitoring with Zabbix Agents and Basic Protocols.

With the files in place, let's proceed to creating items. Navigate to Configuration | Hosts, click on Items next to A test host, then click on Create item. Fill in the following:

- **Name:** First logfile
- **Type:** Zabbix agent (active)
- **Key:** log[/tmp/zabbix_logmon/logfile1]
- **Type of information:** Log
- **Update interval:** 1

![](img/f5313a804532fabacc8d07cc8607eeba_404_0.png)

When done, click on the Add button at the bottom. As mentioned earlier, log monitoring only works as an active item, so we used that item type. For the key, the first parameter is required—it's the full path to the file we want to monitor. We also used a special type of information here, Log. But what about the update interval, why did we use such a small interval of 1 second? For log items, this interval is not about making an actual connection between the agent and the server, it's only about the agent checking whether the file has changed—it does a stat() call, similar to what tail -f does on some platforms/filesystems. Connection to the server is only made when the agent has anything to send in.

With active items, log monitoring is both quick to react, as it is checking the file locally, and also avoids excessive connections. It could be implemented as a somewhat less efficient passive item, but that's not supported yet as of Zabbix 3.0.0.

With the item in place, it should not take longer than 3 minutes for the data to arrive—if everything works as expected, of course. Up to 1 minute could be required for the server to update the configuration cache, and up to 2 minutes could be required for the active agent to update its list of items. Let's verify this—navigate to Monitoring | Latest data and filter by host A test host. Our First logfile item should be there, and it should have some value as well:

| First logfile | 2016-05-09 10:01:05 | 2016-08-13 13:03:05 third log ... |

Even short values are excessively trimmed here. It is hoped that this will be improved in further releases.
If the item is unsupported and the configuration section complains about permissions, make sure permissions actually allow the Zabbix user to access that file. If the permissions on the file itself look correct, check the execute permission on all the upstream directories, too. Here and later, keep in mind that unsupported items will take up to 10 minutes to update after the issue has been resolved.

As with other non-numeric items, Zabbix knows that it cannot graph logs, thus there's a History link on the right-hand side—let's click on it:

| TIMESTAMP | LOCAL TIME | VALUE |
| 2016-05-09 10:01:05 |  | 2016-08-13 13:03:05 third log entry |
| 2016-05-09 10:01:05 |  | 2016-08-13 13:02:04 second log entry |
| 2016-05-09 10:01:05 |  | 2016-08-13 13:01:03 a log entry |

If you see no values in the History mode, it might be caused by a bug in the Zabbix time scroll bar. Try selecting the 500 latest values in the dropdown in the upper-right corner.

All the lines from our log file are here. By default, Zabbix log monitoring parses whole files from the very beginning. That is good in this case, but what if we wanted to start monitoring some huge existing log file? Not only would that parsing be wasteful, we would also likely send lots of useless old information to the Zabbix server. Luckily, there's a way to tell Zabbix to only parse new data since the monitoring of that log file started. We could try that out with our second file, and to keep things simple, we could also clone our first item. Let's return to Configuration | Hosts, click on Items next to A test host, then click on First logfile in the NAME column. At the bottom of the item configuration form, click on Clone and make the following changes:

- **Name:** Second logfile
- **Key:** log[/tmp/zabbix_logmon/logfile2,,,skip]

> There are four commas in the item key – this way we are skipping some parameters and only specifying the first and fifth parameters.

When done, click on the Add button at the bottom. Same as before, it might take up to 3 minutes for this item to start working. Even when it starts working, there will be nothing to see in the latest data page – we specified the skip parameter and thus only any new lines would be considered.

> Allow at least 3 minutes to pass after adding the item before executing the following command below. Otherwise, the agent won't have the new item definition yet.

To test this, we could add some lines to Second logfile. On "A test host", execute:

```
$ echo "2016-08-13 13:04:05 fourth log entry" >> /tmp/zabbix_logmon/logfile2
```

> This and further fake log entries increase the timestamp in the line itself – this is not required, but looks a bit better. For now, Zabbix would ignore that timestamp anyway.

A moment later, this entry should appear in the latest data page:

| Second logfile | 2016-05-09 11:42:15 | 2016-08-13 13:04:05 fourth log... |

If we check the item history, it is the only entry, as Zabbix only cares about new lines now.

> 💡 The skip parameter only affects behavior when a new log file is monitored. While monitoring a log file with and without that parameter, the Zabbix agent does not re-read the file, it only reads the added data.

## Filtering for specific strings

Sending everything is acceptable with smaller files, but what if a file has lots of information and we are only interested in error messages? The Zabbix agent may also locally filter the lines and only send to the server the ones we instruct it to. For example, we could grab only lines that contain the string error in them. Modify the **Second logfile** item and change its key to:

```
log[/tmp/zabbix_logmon/logfile2,error,,,skip]
```

That is, add an error after the path to the log file. Note that now there are three commas between `error` and `skip`—we populated the second item key parameter. Click on **Update**. Same as before, it may take up to 3 minutes for this change to propagate to the Zabbix agent, so it is suggested to let some time pass before continuing. After making a tea, execute the following on "A test host":

```
$ echo "2016-08-13 13:05:05 fifth log entry" >> /tmp/zabbix_logmon/logfile2
```

This time, nothing new would appear in the **Latest data** page—we filtered for the error string, but this line had no such string in it. Let's add another line:

```
$ echo "2016-08-13 13:06:05 sixth log entry - now with an error" >> /tmp/zabbix_logmon/logfile2
```

Checking the history for the `logfile2` logfile item, we should only see the latest entry.

How about some more complicated conditions? Let's say we would like to filter for all error and warning string occurrences, but for warnings only if they are followed by a numeric code that starts with numbers 60-66. Luckily, the filter parameter is actually a regular expression—let's modify the second log monitoring item and change its key to:

```
log[/tmp/zabbix_logmon/logfile2,"error|warning 6[0-6]",,,skip]
```

## Advanced Item Monitoring

We changed the second key parameter to "error|warning 6[0-6]", including the double quotes. This regular expression should match all errors and warnings that start with numbers 60-66. We had to double quote it, because the regexp contained square brackets, which are also used to enclose key parameters. To test this out, let's insert in our log file several test lines:

```sh
$ echo "2016-08-13 13:07:05 seventh log entry – all good" >> /tmp/zabbix_logmon/logfile2

$ echo "2016-08-13 13:08:05 eighth log entry – just an error" >> /tmp/zabbix_logmon/logfile2

$ echo "2016-08-13 13:09:05 ninth log entry – some warning" >> /tmp/zabbix_logmon/logfile2

$ echo "2016-08-13 13:10:05 tenth log entry – warning 13" >> /tmp/zabbix_logmon/logfile2

$ echo "2016-08-13 13:11:05 eleventh log entry – warning 613" >> /tmp/zabbix_logmon/logfile2
```

Based on our regular expression, the log monitoring item should:

- Ignore the seventh entry, as it contains no error or warning at all
- Catch the eighth entry, as it contains an error
- Ignore the ninth entry — it has a warning, but no number following it
- Ignore the tenth entry — it has a warning, but the number following it does not start within range of `60-66`
- Catch the eleventh entry — it has a warning, the number starts with 61, and that is in our required range, `60-66`

Eventually, only the eighth and eleventh entries should be collected. Verify that in the latest data page only the entries that matched our regexp should have been collected.

The regexp we used was not very complicated. What if we would like to exclude multiple strings or do some other, more complicated filtering? With the POSIX EXTENDED regular expressions that could be somewhere between very complicated and impossible. There is a feature in Zabbix, called **global regular expressions**, which allows us to define regexps in an easier way. If we had a global regexp named `Filter logs`, we could reuse it in our item like this:

```text
log[/tmp/zabbix_logmon/logfile2,@Filter logs,,,skip]
```

Global regular expressions are covered in more detail in *Chapter 12, Automating Configuration.*
