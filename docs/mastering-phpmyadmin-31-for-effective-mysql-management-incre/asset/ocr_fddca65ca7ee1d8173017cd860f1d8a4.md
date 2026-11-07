# Renaming a database

A Rename database to dialog is available. Although this operation is not directly supported by MySQL, phpMyAdmin does it indirectly by creating a new database, renaming each table (thus sending it to the new database), and dropping the original database.

# Copying a database

It is also possible to do a complete copy of a database, even if MySQL itself does not support this operation natively.

![](bbox=[0.28, 0.32, 0.72, 0.51])

# Summary

This chapter covers the operations we can perform on entire tables or databases. It also looks at table maintenance operations for table repair and optimization, changing various table attributes, table movements (including renaming and moving to another database), and multi-table operations. In the next chapter, we will begin to examine advanced features that rely on the linked-tables infrastructure, such as the relational system.