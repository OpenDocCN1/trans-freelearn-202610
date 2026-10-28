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