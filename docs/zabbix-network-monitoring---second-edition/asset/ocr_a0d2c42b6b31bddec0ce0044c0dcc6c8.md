These values should be kept reasonably low. Keeping data for a long period of time will increase the database size, and that can impact the performance a lot.

What about the history and trend settings in here? While they're configurable per item normally, we may override those here. Also, for each of the entries, internal housekeeping may be disabled. These options are aimed at users who have to manage large Zabbix installations. When the database grows really large, its performance significantly degrades, and can be improved by partitioning the biggest tables—splitting them up by some criteria. With Zabbix, it is common to partition the history and trends tables, sometimes adding events and alerts tables. If partitioning is used, parts of tables (partitions) are removed, and the internal housekeeping for those tables should be disabled. A lot of people in the Zabbix community will eagerly suggest partitioning at the first opportunity. Unless you plan to have a really large installation or know database partitioning really well, it might be better to hold off. There is no officially supported or built-in partitioning scheme yet, and one might appear in the future. If it does and your partition scheme is different, it will be up to you to synchronize it with the official one.

# Who did that?

"Now who did that?"—a question occasionally heard in many places, IT workplaces included. Weird configuration changes, unsolicited reboots—accountability and a trace of actions help a lot to determine whether the questioner was the one who made the change and then forgot about it. For Zabbix configuration changes, an internal audit log is available. Just like most functionality, it is conveniently accessible from the web frontend. During our configuration quest, we made quite a lot of changes—let's see what footprints we left. Navigate to **Reports | Audit**, and set the filter time bar to a period that approximately matches the initial installation of this Zabbix instance. We are presented with a list of the things we did, although you can also only see logging in and out in the first page of the audit records:

| TIME | USER | IP | RESOURCE | ACTION | ID | DESCRIPTION | DETAILS |
|---|---|---|---|---|---|---|---|
| 2015-12-12 14:06:12 | Admin | 127.0.0.1 | User | Login | 1 |  |  |
| 2015-12-12 14:06:12 | Admin | 127.0.0.1 | User | Logout | 1 |  | Manual Logout |
| 2015-12-12 14:05:11 | Admin | 127.0.0.1 | User | Login | 1 |  |  |
| 2015-12-12 14:05:11 | Admin | 127.0.0.1 | User | Logout | 1 |  | Manual Logout |
| 2015-12-12 14:04:10 | Admin | 127.0.0.1 | User | Login | 1 |  |  |
| 2015-12-12 14:04:10 | Admin | 127.0.0.1 | User | Logout | 1 |  | Manual Logout |