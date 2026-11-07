It is also possible to see the following screen:

![](bbox=[0.41, 0.17, 0.59, 0.24])

This means that our current MySQL rights do not allow us to see any existing databases.

> A MySQL server always has at least one database (named **mysql**), but in this case, we do not have the right to see it. Moreover, as MySQL 5.0.2, a special database called **information_schema** appears at all times in the database list. It contains a set of views describing the metadata visible for the logged-in user.

We may have the right to create one, as explained in Chapter 4.

# Light mode

The left panel can be shown in two ways—the Light mode and the Full mode. The Light mode is used by default, defined by a TRUE value in $cfg['LeftFrameLight']. This mode shows a drop-down list of the available databases, and only tables of the currently chosen database are displayed. It is more efficient than Full Mode; the reason is explained in the Full Mode section appearing later in the chapter. Here we have chosen the **mysql** database:

![](bbox=[0.42, 0.56, 0.58, 0.83])