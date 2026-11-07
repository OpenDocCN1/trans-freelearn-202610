To get rid of this error message, we go back to the **Structure** for the book table and remove the full-text index on the **description** field. While we are on this screen, let's also remove the combined index we created on **author_id** and **language**. We will do this because we want to see the consequences of a missing index later in this chapter. At this point, we are able to switch the book table to InnoDB.

The foreign key system in InnoDB maintains integrity between the related tables. Hence, we cannot add a non-existent author ID to the book table. In addition, actions are programmable when DELETE or UPDATE operations are performed on the master table (in our case, book).

Opening the book table on its **Structure** subpage and entering the **Relation view**, now displays a different page:

![](bbox=[0.17, 0.35, 0.83, 0.59])

This page tells us:

+   - We have an internal relation defined for **author_id** to the author table.
+   - We don't yet have any InnoDB relations defined.
+   - We will be able to remove the internal relation, when the same relation has been defined in InnoDB. Indeed, phpMyAdmin advises us that: "**An internal relation is not necessary when a corresponding FOREIGN KEY relation exists**". So, it would be better to remove it.
+   - **ON DELETE** and **ON UPDATE** options are available for InnoDB relations.