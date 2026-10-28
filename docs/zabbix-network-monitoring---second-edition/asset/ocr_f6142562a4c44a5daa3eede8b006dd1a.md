# Advanced Item Monitoring

That would call the init script, discard all stdin and stderr output (because we only want to return a single number to Zabbix), and return "1" upon a non-successful exit code. That should work, right? There's only one huge problem—parameters should never return an empty string, which is what would happen with such a check if Postfix was running. If the Zabbix server were to check such an item, it would assume the parameter is unsupported and deactivate it as a consequence. We could modify this string so that it becomes the following:

```
/etc/init.d/postfix status  > /dev/null 2>&1 && echo 0 || echo 1
```

This would work very nicely, as now a Boolean is returned and Zabbix always gets valid data. But there's a possibly better way. As the exit code is 0 for success and nonzero for problems, we could simply return that. While this would mean that we won't get nice Boolean values only, we could still check for nonzero values in a trigger expression like this:

```
{hostname:item.last()}<>0
```

As an added benefit, we might get a more detailed return message if the init script returns a more detailed status with nonzero exit codes. As defined by the **Linux Standard Base**, the exit codes for the status commands are the following:

| Code | Meaning |
|---|---|
| 0 | Program is running or service is OK |
| 1 | Program is dead and `/var/run` pid file exists |
| 2 | Program is dead and `/var/lock` lock file exists |
| 3 | Program is not running |
| 4 | Program or service status is unknown |

There are several reserved ranges that might contain other codes, used by a specific application or distribution—those should be looked up in the corresponding documentation.

For such a case, our user parameter command becomes even simpler, with the full string being the following:

```
UserParameter=service.status[*],/etc/init.d/"$1" status > /dev/null 2>&1; echo $?
```