# Inspecting all the configuration parameters

You can inspect all the configuration parameters from a live system by issuing a query against the pg_settings special catalog. This catalog contains every setting that the current version of PostgreSQL will accept, along with default values, current values, a description of the parameter aim, and much more.

As an example, with the following query, you can gather information about every configuration parameter, including their short and long descriptions, default values, and current values:

```
forumdb=> SELECT name, setting || ' ' || unit AS current_value, short_desc, extra_desc, min_val, max_val, boot_val, reset_val FROM pg_settings;
...
name          | authentication_timeout
current_value | 360 s
short_desc    | Sets the maximum allowed time to complete client authentication.
extra_desc    |
min_val       | 1
max_val       | 600
boot_val      | 60
reset_val     | 360
```

In the preceding example, the authentication_timeout has been set to 360 seconds, as indicated by the setting column, and its value can be tuned in the range of 1 second to 600 seconds; boot_val is the default value that the parameter will assume if it is not configured at all, and it is set to 60 seconds. Similarly, the reset_val column indicates the value the parameter will assume if it is reset after a change.

The pg_settings special catalog also contains other useful information, including the file and the line number from where a parameter has been loaded. This information can be used to quickly find where a configuration parameter has been set in the postgresql.conf file or another configuration file. As an example, the following query will show where each parameter has been loaded from:

```
forumdb=# SELECT name, setting AS current_value, sourcefile, sourceline, pending_restart FROM pg_settings;
...
name          | log_destination
```