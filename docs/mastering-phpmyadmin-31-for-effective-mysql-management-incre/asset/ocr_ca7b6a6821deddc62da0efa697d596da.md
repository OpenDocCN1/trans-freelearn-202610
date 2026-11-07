After the sort has been done on **author_id**, books for author **1** will be displayed first, followed by the books for author **2**, and so on (we are talking about a default browsing of the table without explicit sorting). We can also specify the sort order as **Ascending** or **Descending.**

If we insert another row, describing a new book from author **1**, and then click **Browse**, the book will not be displayed along with the other books for this author because the sort was done before the insertion.

# Table collation

To better see the issues of collation change at various levels, let's first create a new author with a character **é** in his name:

![](bbox=[0.215, 0.365, 0.785, 0.46])

The name column currently has a **latin1_swedish_ci** collation, as can be seen via the **Structure** page. On the **Operations** page, if we change the collation for table author from **latin1_swedish_ci** to, say, **utf8_general_ci**, this generates:

```
ALTER TABLE `author` DEFAULT CHARACTER SET utf8 COLLATE
utf8_general_ci
```

Therefore, we only changed the default collation for future columns that will be added to this table.

In an effort to further test collation changes, we can go to **Structure** page and change the collation of author's **name** column to **utf8_general_ci** (by clicking the **Change** icon). Having done that, we browse the **author** table and still see correctly the **é** character, because MySQL updated the contents of this column on collation change.