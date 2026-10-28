
## The upgrade process

Read the upgrade notes.

What was that? Yes, before performing any upgrades, take a little time, go to the Zabbix manual, and read the upgrade notes. If you are jumping over a few major versions, do read all the upgrade notes in between. Even if you have followed Zabbix development a bit, you might have missed some change that could cause problems—removed or added configuration parameters, memory requirement changes, API changes...upgrade notes should list all significant changes.

It is also highly suggested to read the pages on new features and improvements, called **What's new**. While it's much less risky to miss some of those changes, knowing about them could help you use Zabbix in a more efficient way.

Let's talk about the upgrade process itself now. This process and compatibility will differ depending on the version change you are performing:

- A minor version upgrade inside the same major version is simple and easy to undo
- A major version upgrade is more complicated and hard or impossible to undo

## Minor version upgrade

This is the simplest case. For example, upgrading from 3.0.0 to 3.0.1 or from 3.0.1 to 3.0.5 would be considered a minor version upgrade.

> Zabbix uses the third number to denote a minor version.

When performing a minor version upgrade, we may upgrade any combination of components—server, agents, proxies, Java gateway, and so on. While it is suggested to keep the main components of the same version to reduce confusion, a 3.0.0 server will happily work with a 3.0.1 frontend, 3.0.2 proxies, and 3.0.3 agents. Inside one major version, all components are compatible with each other.

It is also perfectly fine to skip minor versions when upgrading—as mentioned, going from 3.0.1 directly to 3.0.5 is perfectly fine.

While minor versions won't have upgrade notes often, do make sure to check for them. And read those **What's new** pages.

You may keep and use multiple versions of the Zabbix frontend in parallel, as long as they all are of the same major version. While normally not needed, it can be very helpful when some debugging or comparison has to be performed.

## Major-level upgrades

A major-level upgrade is slightly different from a minor version upgrade. As a quick reminder, definitely go and read the upgrade notes—major versions will always have some. Remember about the What's new pages, too.

Back to the major version upgrade itself, the most significant differences from a minor version upgrade are as follows:

- Database schema changes
- Compatibility
- Reading the upgrade notes

> When performing major-level upgrades from source, it is suggested to avoid copying the new frontend files over the old files. Leftover files from the old version might cause problems.

Let's talk about database schema changes right now, let's discuss compatibility in detail a bit later, and let's always remember to read our upgrade notes.

While the Zabbix team works hard to keep minor version upgrades without database changes, for major releases, it's open season. Changes to the database schema and its contents are made to accommodate new features, improve performance, and increase flexibility. Users wouldn't appreciate it if they couldn't keep gathered data and created configuration in the name of new greatness, so with each new version, a database upgrade patch is provided. This may include adding new tables and columns, removing tables and columns, and changing the data layout.

Given that a major version upgrade changes the database, make sure you have a recent backup. While upgrades are extensively tested, it is not possible for the developers to test all scenarios. What has worked for a thousand people might break in some obscure way for you. Additionally, interrupting the upgrade process because of a hardware or electricity failure is likely to leave your database in a broken state. You have been warned, so get that backup ready.

You are strongly encouraged to test the upgrade on a test installation, preferably using a production dataset (maybe with trimmed history and trend data, if the available hardware does not permit testing with a full copy).

With a fresh backup created, we are ready to engage the major version upgrade. The database upgrade process significantly changed for Zabbix version 2.2. In older versions, we had to apply the database patch manually. If you happen to have an old Zabbix installation—old being pre-2.0—you will have to patch it up to the 2.0 database schema manually. For your reference, the database patches are located in the upgrades/dbpatches directory in the source tree, but if you really want to follow that path, make sure to consult with the Zabbix community via the channels discussed in Appendix B, Being Part of the Community.

For upgrading to Zabbix 2.0 or more recent versions, no manual patching is required. Starting up the new server will automatically upgrade the database schema. Note that this database upgrading happens without a confirmation. Be careful not to start a more recent server binary against an older database version if you do not intend to change the database.

One last note regarding the upgrade notes: promise. While the latest Zabbix upgrades are really quick even in large installations, older versions sometimes upgraded historical data tables, and that took a long time—like, really, a long time. In some reported cases, it was days. If such a change is required in any of the future versions, it will be mentioned in the upgrade notes, and you'll be glad you read them.

## Database versioning

With all this talk about the database version and schema changes, let's take a closer look at how version information is stored and how we can check the upgrade status. Examine the dbversion table in your Zabbix database:

```
mysql> select * from dbversion;
+----------------+-------------+
| mandatory | optional |
+----------------+-------------+
| 3000000 | 3000000 |
+----------------+-------------+
1 row in set (0.00 sec)
```

This table is the way Zabbix components determine which version of the database schema they are dealing with. There are two numbers in there: the mandatory and optional version. The following rules are important regarding version numbers:

- Inside one major version, the mandatory version number is always the same
- If a more recent server is started, it upgrades the database to the latest mandatory and optional version
- The server and frontend can work with a database as long as its mandatory version matches their mandatory version exactly—the optional version does not affect compatibility

The mandatory version encodes things such as table changes, column changes, and otherwise significant changes that break compatibility. The optional version would usually denote an index change—something that is helpful but does not prohibit older versions from working with a more recent database.

> Zabbix server can upgrade to the latest database schema version on all versions from 2.0 onwards. To upgrade the database from version 2.0 to 3.2, it is not required to use server versions in succession—it is enough to start server version 3.2.

When a new major version of Zabbix Server is started, it is possible to observe the current status and database upgrade progress in the server log file:

```
10852:20151209:094918.686 Starting Zabbix Server. Zabbix 3.0.0
(revision {ZABBIX_REVISION}).
10852:20151209:094918.729 ***** Enabled features *****
...
10852:20151209:094918.730 TLS support:                    NO
10852:20151209:094918.730 *******************************
10852:20151209:094918.730 using configuration file: /usr/local/etc/
zabbix_server.conf
10852:20151209:094918.820 current database version (mandatory/
optional): 3000000/ 3000000
10852:20151209:094918.820 required mandatory version:  3000000
10852:20151209:094918.820 starting automatic database upgrade
...
10852:20151209:094918.866 completed 20% of database upgrade
...
10852:20151209:094918.937 completed 100% of database upgrade
10852:20151209:094918.937 database upgrade fully completed
```

Notice how it prints out the current mandatory and optional database versions we just examined in the database and the required mandatory version. If the mandatory or optional database version numbers are lower than the required version, the server will upgrade the database. If the database mandatory version is higher than the server version, the server will refuse to start up. During the database schema upgrade, no monitoring happens. Monitoring restarts once the database upgrade is complete.
