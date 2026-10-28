If the item receives error lines only, we could use an expression like the one above, but we could also simplify it. In this case, just having any value is a problem situation, so we would use the reversed nodata() function again and alert on values being present:

```
{host.item.nodata(10m)}=0
```

Here, if we have any values in the last 10 minutes, that's it—it's a PROBLEM. No values, the trigger switches to OK. This is somewhat less resource intensive as Zabbix doesn't have to evaluate the actual item value.

Yet another trigger function that we could use here is count(). It would allow us to fire an alert when there's a certain number of interesting strings—such as errors—during some period of time. For example, the following will alert if there are more than 10 errors in the last 10 minutes:

```
{host.item.count(10m,error,like)}>10
```

# Extracting part of the line

Sometimes we only want to know that an error was logged. In those cases, grabbing the whole line is good enough. But sometimes the log line might contain an interesting substring, maybe a number of messages in some queue. A log line might look like this:

```
2015-12-20 18:15:22 Number of messages in the queue: 445
```

Theoretically, we could write triggers against the whole line. For example, the following regexp should match when there are 10,000 or more messages:

```
messages in the queue: [1-9][0-9]{4}
```

But what if we want to have a different trigger when the message count exceeds 15,000? That trigger would have a regexp like this:

```
messages in the queue: (1[5-9]|[2-9].)[0-9]{3}
```

And if we want to exclude values above 15,000 from our first regexp, it would become the following:

```
messages in the queue: 1[0-4][0-9]{3}
```