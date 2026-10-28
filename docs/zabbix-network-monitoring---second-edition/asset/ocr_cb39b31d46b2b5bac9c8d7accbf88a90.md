# Automating Configuration

This feature is really nice, but properly explaining it seems to be complicated, so here's a schematic. Without context, user macros were evaluated as in the right-hand column — that is, the host level was checked first, then template, and then global. With context, it is the same—just that the macro name with context is looked up in all three levels first, then we fall back to the macro name without context on all three levels. The first place where there's a match will determine the value for that macro.

![](bbox=[0.097, 0.215, 0.904, 0.463])

When used in triggers like this, this feature allows us to have different thresholds for different filesystems—and that can also be customized per host. We could have a user macro {$FS_FREE_THRESHOLD:/home} set to 20 on one host, 30 on another, and so on.

Of course, this is not limited to triggers—it is supported in all the locations where user macros are supported, including item-key parameters and trigger-function parameters. A trigger could check the average temperature for 5 minutes on one system and 15 minutes on another.

# CPU discovery

Yet another discovery method supported by the Zabbix agent is **CPU discovery**. It returns all CPUs (or cores) present on a system. Now that we know how to get the LLD JSON, we only need to know which item key is used to return CPU information—that's `system.cpu.discovery`. Run this on a test host:

```
$ zabbix_get -s 127.0.0.1 -k system.cpu.discovery | json_pp
```