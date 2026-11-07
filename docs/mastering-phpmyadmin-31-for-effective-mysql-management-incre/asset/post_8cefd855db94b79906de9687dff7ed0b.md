
After the sort has been done on **author_id**, books for author **1** will be displayed first, followed by the books for author **2**, and so on (we are talking about a default browsing of the table without explicit sorting). We can also specify the sort order as **Ascending** or **Descending.**

If we insert another row, describing a new book from author **1**, and then click **Browse**, the book will not be displayed along with the other books for this author because the sort was done before the insertion.

## Table collation

To better see the issues of collation change at various levels, let's first create a new author with a character **é** in his name:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_189_0.png)

The name column currently has a **latin1_swedish_ci** collation, as can be seen via the **Structure** page. On the **Operations** page, if we change the collation for table author from **latin1_swedish_ci** to, say, **utf8_general_ci**, this generates:

```
ALTER TABLE `author` DEFAULT CHARACTER SET utf8 COLLATE
utf8_general_ci
```

Therefore, we only changed the default collation for future columns that will be added to this table.

In an effort to further test collation changes, we can go to **Structure** page and change the collation of author's **name** column to **utf8_general_ci** (by clicking the **Change** icon). Having done that, we browse the **author** table and still see correctly the **é** character, because MySQL updated the contents of this column on collation change.

## Impact of switching connection collation

Changing the connection collation (from the homepage collation dialog) can have unexpected effects. Let's switch it to latin1_bin and browse our author table again:

|  |  |  | id | name | phone |
|---|---|---|---|---|---|
| ☐ | ✏️ | ❌ | 1 | John Smith | +01 445 789-1234 |
| ☐ | ✏️ | ❌ | 2 | Maria Sunshine | 333-3333 |
| ☐ | ✏️ | ❌ | 3 | Andr♦ Smith | 888-9999 |

The data encoded in UTF-8 in the database is now (wrongly) interpreted as being in latin1, introducing an inconsistency in the display. Therefore, we have to be careful with our collation changes, and you should revert the collation changes made in this section and the previous one.

## Table options

Other attributes that influence the table's behavior may be specified using the Table options dialog:

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_190_0.png)

The options are:

- **PACK_KEYS**: Setting this attribute results in a smaller index. This can be read faster but takes more time to update. Available for the MyISAM storage engine.
- **CHECKSUM**: This makes MySQL compute a checksum for each row. This results in slower updates, but finding of corrupted tables becomes easier. Available for MyISAM only.
- **DELAY_KEY_WRITE**: This instructs MySQL not to write the index updates immediately, but to queue them for writing later. This improves performance. Available for MyISAM only.
- **ROW_FORMAT**: To the storage engines that support this feature (MyISAM, InnoDB, PBXT, and Maria), a choice of row format is presented. The default value being the current state of this table's row format.
- **TRANSACTIONAL, PAGE_CHECKSUM**: Applies to the Maria storage engine. Currently documented at http://dev.mysql.com/doc/refman/5.1-maria/en/se-maria-tableoptions.html.
- **auto-increment**: This changes the auto-increment value. It is shown only if the table's primary key has the auto-increment attribute.

## Renaming, moving, and copying tables

The **Rename** operation is the easiest to understand — the table simply changes its name and stays in the same database.

The **Move** operation (shown in the following screen) can manipulate a table in two ways — change its name and also the database in which it is stored.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_191_0.png)

Moving a table is not directly supported by MySQL. So, phpMyAdmin has to create the table in the target database, copy the data, and then finally drop the source table.

The **Copy** operation leaves the original table intact and copies its structure or data (or both) to another table, possibly in another database. Here, the **book-copy** table will be an exact copy of the book source table. After the copy, we remain in the Table view for the book table, unless we select **Switch to copied table**.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_191_1.png)

The **Structure only** copy is done to create a test table with the same structure.

## Appending data to a table

The Copy dialog may also be used to append (add) data from one table to another. Both tables must have the same structure. This operation is achieved by entering the table to which we want to copy the data and choosing Data only.

For example, we would want to append data when book data, coming from various sources (various publishers), is stored in more than one table, and we want to aggregate all the data to one place. For MyISAM, a similar result can be obtained by using the Merge storage engine (which is a collection of identical MyISAM tables). However, if the table is InnoDB, we need to rely on phpMyAdmin's Copy feature.

## Multi-table operations

In the Database view, there is a checkbox next to each table name and a drop-down menu under the table list. This enables us to quickly choose some tables and perform an operation on all those tables at once. Here, we select the book-copy and the book tables, and choose the Check table operation for the selected tables.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_192_0.png)

We could also quickly select or deselect all the checkboxes with Check All / Uncheck All.

## Repairing an "in use" table

The multi-table mode is the only method (unless we know the exact SQL query to type) for repairing a corrupted table. Such tables may be shown with the in use flag in the database list. Users seeking help in the support forums for phpMyAdmin often receive this tip from experienced phpMyAdmin users.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_193_0.png)

## Database operations

The Operations tab in the Database view gives access to a panel that enables us to perform operations on a database taken as a whole.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_193_1.png)

## Renaming a database

A Rename database to dialog is available. Although this operation is not directly supported by MySQL, phpMyAdmin does it indirectly by creating a new database, renaming each table (thus sending it to the new database), and dropping the original database.

## Copying a database

It is also possible to do a complete copy of a database, even if MySQL itself does not support this operation natively.

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_194_0.png)

## Summary

This chapter covers the operations we can perform on entire tables or databases. It also looks at table maintenance operations for table repair and optimization, changing various table attributes, table movements (including renaming and moving to another database), and multi-table operations. In the next chapter, we will begin to examine advanced features that rely on the linked-tables infrastructure, such as the relational system.

## 11

## The Relational System

Welcome to the section of the book where we start to cover advanced features. The relational system allows users to work more closely with phpMyAdmin, as we will see in the following chapters. This chapter explains how to define inter-table relations. It also explains how to install the linked-tables infrastructure — a prerequisite for the advanced features.

## Relational MySQL

When application developers use PHP and MySQL to build web interfaces or other data manipulation applications, they usually establish relations between tables using the underlying SQL queries. Examples of this would be: "get an invoice and all its items" and "get all books by an author".

In the first versions of phpMyAdmin, MySQL stored information about which table belonged to which database. However, the relational data structure (how tables relate to each other) was not stored into MySQL. Relations were temporarily made by the applications to generate meaningful results. In other words, the relations were in our head.

This was considered a shortcoming of MySQL by phpMyAdmin developers and users. Hence, the team started to build an infrastructure to support relations for MyISAM tables. The infrastructure evolved to support a growing array of special features. We can describe this infrastructure as metadata, that is, data about data.

phpMyAdmin 2.2.0 already had the bookmarks feature, that is, the ability to recall frequently used queries (described in Chapter 14), and version 2.3.0 generalized the metadata system. Subsequent versions were built on this facility. Examples of this would be: the 2.5.x family with its MIME-based transformations (as described in Chapter 16) and the 2.10.x family with a graphical relation builder called Designer (described in this chapter).
