For this, the $cfg['LeftDisplayServers'] parameter must be set to TRUE. The list of servers can have two forms—a drop-down list or links. Which form appears depends on $cfg['DisplayServersList']. By default, this parameter is set to FALSE, so we see a drop-down list of servers. Setting $cfg['DisplayServersList'] to TRUE produces a list of links to all defined servers:

![](bbox=[0.38, 0.24, 0.61, 0.4])

# Handling many databases or tables

This section describes some techniques to cope with a server holding a huge number of databases and tables.

# Limits on the interface

Before phpMyAdmin 2.11, it was difficult to work with the interface if we had access to hundreds or even thousands of databases, or hundreds of tables in the same database. Loading of the navigation panel was slow, or did not work at all. In 2.11, the interface has been reworked to take care of this.

Two new parameters, shown here with their default values, establish a limit on the number of databases and tables displayed, by adding a page selector and navigation links:

```
$cfg['MaxDbList'] = 100;
$cfg['MaxTableList'] = 250;
```

The effect of $cfg['MaxDbList'] can be seen on the main panel in Server view: