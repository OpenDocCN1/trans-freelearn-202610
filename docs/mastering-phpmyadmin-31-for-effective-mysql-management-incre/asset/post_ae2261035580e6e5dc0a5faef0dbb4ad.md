
## Information_schema

In the SQL:2003 standard, access to the data dictionary (or database metadata) is provided by a structure called information_schema. As this is part of the standard, and already exists in other database systems, the decision to implement this feature into MySQL was a very good one.

> MySQL has added some information that is not part of the standard—for example, INFORMATION_SCHEMA.COLUMNS.COLUMN_TYPE. Beware of the fact that if you plan to use this information in a software project, it has to remain portable to other SQL implementations.

A phpMyAdmin user sees the information_schema as a normal database containing views that describe many aspects of the structure of the databases hosted on this server. Here is a subset of what can be seen (and in fact, the only possible operation on this database is SELECT).

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_291_0.png)

Internally, phpMyAdmin calls the information_schema for some of its operations instead of the corresponding SHOW statements to retrieve metadata. However, phpMyAdmin developers have noticed—and this was confirmed by other developers at the 2007 MySQL Users Conference—that some SELECT operations involving a WHERE clause on information_schema are really slow (many minutes of wait time) when the server hosts hundreds of databases or tables. Therefore, in the current state of MySQL, caution must be exercised about using this feature in our programming projects.

The $cfg['Servers'][$i]['hide_db'] parameter can be used to hide this "database" to users who might be confused by the sudden apparition of a database they know nothing about. It will probably depend on their level of expertise in MySQL. On a multi-user installation of phpMyAdmin, we cannot please everyone about this parameter's value.

## Profiling

Profiling support has been added in the MySQL versions 5.0.37 and 5.1.28. We have previously seen the Profiling checkbox appear in query results.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_292_0.png)

When this box is checked, phpMyAdmin will analyze every query (including the current one), and a report about the execution time of each MySQL internal operation is displayed.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_292_1.png)

Although the profiling system can report additional information about operations (such as, the CPU time, and even the internal server's function names), phpMyAdmin currently displays only the name of the operation and its duration.

## Partitioning

User-defined partitioning (http://dev.mysql.com/doc/refman/5.1/en/partitioning.html) is offered in MySQL 5.1. Using this feature in phpMyAdmin requires knowledge of its syntax because there are many partition types. Also, for each partition type, the number of partitions and the values associated with each partition are too random to be easily represented on a web interface.

## Table creation

Let's try it by creating a table named test with one field. On the table creation panel, if connected to a MySQL 5.1 server, phpMyAdmin shows a PARTITION definition dialog:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_293_0.png)

Here, we type in a PARTITION BY RANGE clause, which will create partitions on the id column.

## Operations

On a table on which a partition has been defined, the Operations subpage displays a Partition maintenance dialog where we can:

- Choose a partition and then ask for an action such as Rebuild on it
- Remove all the partitioning

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_294_0.png)

## Exporting

Finally, exporting this test table in SQL mode produces statements with embedded comments that a MySQL 5.1 server would recognize and interpret to recreate the same partitions.

```
CREATE TABLE `test` (
  `id` int(11) NOT NULL
) ENGINE=MyISAM DEFAULT CHARSET=latin1
/*!50100 PARTITION BY RANGE (id)
(PARTITION p0 VALUES LESS THAN (1000) ENGINE = MyISAM,
 PARTITION p1 VALUES LESS THAN (2000) ENGINE = MyISAM,
 PARTITION p2 VALUES LESS THAN (3000) ENGINE = MyISAM) */;
```

## Event scheduler

The Event Scheduler (http://dev.mysql.com/doc/refman/5.1/en/events.html), another new feature of MySQL 5.1, permits the creation of tasks that will run automatically according to a schedule. The schedule is quite flexible and permits, for example, a statement to be run every ten seconds, starting from midnight, May 18, 2011. These can be one-time events or recurring ones.

## Activating the scheduler

We should first verify whether the scheduler is active on our server. If not, we need to activate it. Otherwise, nothing will happen! We start by typing this statement in the query box:

```
SHOW VARIABLES LIKE `event%`;
```

Next, we look for a variable named event_scheduler. If this variable is set to OFF, we need to ask the system administrator (or someone with the SUPER privilege) to execute the following statement:

```
SET GLOBAL event_scheduler = ON;
```

## Granting EVENT permission

Every user who wants to create or drop an event needs the EVENT privilege, either globally, or on the database on which he/she plans to add the event. Please refer to Chapter 18 for details about granting such privileges.

## Creating an event

The current 3.1.x phpMyAdmin version does not have a web interface on which we could choose the various parts of the CREATE EVENT statement. Therefore, the only method left is to use the SQL query box to enter the statement and to know its syntax! Here, we'll use a totally fictitious example:

```
CREATE EVENT add_page_count
    ON SCHEDULE
        EVERY 1 MINUTE
    DO
        UPDATE author set total_page_count = total_page_count + 1 WHERE id = 1;
```

You can now get some amusement by browsing the author table once in a while, and see the counter incrementing for author 1.

## Event manipulation

Events are related to one database, which is why you see an Events slider on the Structure page for the marc_book database. Activating it reveals this panel:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_295_0.png)

Indeed, this is a recurring event. We can use the first icon to edit the event (which will have the effect of deleting and recreating the event), and the X icon to remove it.

## Exporting

It is possible to generate event-related statements at the end of an SQL database export by putting a check mark next to the Add CREATE PROCEDURE/ FUNCTION / EVENT option. Please remember that some events may have an ending moment; Hence, they may have vanished between the time you create them and the time you attempt an export!

## BLOB streaming

Starting with version 3.1.0, phpMyAdmin supports the Scalable BLOB streaming infrastructure for MySQL. The http://blobstreaming.org website explains the goals of this project. Please note that some of its documentation relates to command-line utilities which have few or no use in the context of a web interface such as phpMyAdmin. You should be familiar with BLOB streaming terms, such as repository and field references, before reading further.

In this section, we will use this technology to upload a movie and watch it after streaming from within phpMyAdmin.

## System requirements

The chosen method of implementation in phpMyAdmin relies on all of these components:

- MySQL 5.1
- PBXT (PrimeBase XT storage engine)
- PBMS (PrimeBase Media Streaming)

PBXT is a streaming enabled storage engine. Although other non-streaming enabled storage engines are supported via PBMS UDFs, this mechanism is not implemented in phpMyAdmin.

The presence of the required components can be verified from the phpMyAdmin's homepage, by clicking the Engines link and looking for these lines:

```
PBXT / High performance, multi-versioning transactional engine
PBMS / The Media Stream engine for MySQL
```

In case these lines are not present, please follow the instructions on http://www.primebase.org to install PBXT, or on the BLOB Streaming website to install PBMS.

> You won't see PBMS in the storage engine choices of the **Operations** subpage for a table. PBMS is not a storage engine that can be associated with the structure of a table.

## Configuration

Some PBMS system variables have an impact on BLOB streaming behaviour, and can be defined in `config.inc.php`. More details about them on http://blobstreaming.org/documentation/#sysvar. The recommended values to be put in your server's definition are:

```
$cfg['Servers'][$i]['bs_garbage_threshold'] = 50;
$cfg['Servers'][$i]['bs_repository_threshold'] = '32M';
$cfg['Servers'][$i]['bs_temp_blob_timeout'] = 600;
$cfg['Servers'][$i]['bs_temp_log_threshold'] = '32M';
```

## Implementation limitations in phpMyAdmin

In the current 3.1.1 version of phpMyAdmin, a few limitations exist, with respect to using BLOB streaming. The first limitation has to do with the MySQL server. On a server which has just started, the `pbms_field_references` global MySQL variable may be set to **off**. As field references are used extensively for BLOB referencing, phpMyAdmin tries to set the parameter to on, and this can be done only by a user possessing the SUPER privilege. Therefore, such a user must log in once via phpMyAdmin to set this parameter to the appropriate value, and this will have to be repeated on each restart of the MySQL server.

The second limitation is related to the upload mechanism used to send data to the BLOB repository. The data has to travel first from your workstation to the web server, and then from the web server to the MySQL server. The first travel occurs via a normal HTTP upload. Please refer to Chapter 8, *Importing Structure and Data*, in the *Limits for the transfer* section, for more details on the requirements for uploading. To avoid being hindered by this limit, you can use the web server upload directory (also described in Chapter 8).

> An important note about the host name is due here: In `config.inc.php`, your server name in `$cfg['Servers'][$i]['host']` should be a **FQDN (fully-qualified domain name)**. It should not be something like "localhost"; otherwise, the links used to retrieve streaming data will not work.
