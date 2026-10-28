# Reloading the configuration cache

We met the configuration cache in Chapter 2, Getting Your First Notification, and we discussed ways to monitor it in Chapter 22, Zabbix Maintenance. While it helps a lot performance-wise, it can be a bit of a problem if we are trying to quickly test something. It is possible to force Zabbix server to reload the configuration cache. Run the following to display Zabbix server options:

```
# zabbix_server --help
```

> We briefly discussed Zabbix proxy configuration cache reloading in Chapter 19, Using Proxies to Monitor Remote Locations.

In the output, look for the runtime control options section:

```
-R --runtime-control runtime-option  Perform administrative functions
Runtime control options:
    config_cache_reload  Reload configuration cache
```

Thus, reloading the server configuration cache can be initiated by the following:

```
# zabbix_server --runtime-control config_cache_reload
```

```
zabbix_server [2682]: command sent successfully
```

Examining the server log file will reveal that it has received the signal:

```
forced reloading of the configuration cache
```

In the background, the sending of the signal happens like this:

+   1. The server binary looks up the default configuration file.
+   2. It then looks for the file specified in the PidFile option.
+   3. It sends the signal to the process with that ID.

As discussed in Chapter 19, Using Proxies to Monitor Remote Locations, the great thing with this feature is that it's also supported for active Zabbix proxies. Even better, when an active proxy is instructed to reload its configuration cache, it connects to the Zabbix server, gets all the latest configuration, and then reloads the local configuration cache. If such a signal is sent to a passive proxy, it ignores the signal.