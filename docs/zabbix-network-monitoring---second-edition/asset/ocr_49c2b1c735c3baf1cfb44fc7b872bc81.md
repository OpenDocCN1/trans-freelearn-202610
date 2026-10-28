# Working Closely with Data

The output will show us that each record in this table contains four fields (your output will have different values):

```text
+--------+------------+--------+----------+
| itemid | clock      | value  | ns       |
+--------+------------+--------+----------+
|  23668 | 1430700808 | 0.0000 | 644043321 |
|  23669 | 1430700809 | 0.0000 | 644477514 |
|  23668 | 1430700838 | 0.0000 | 651484815 |
+--------+------------+--------+----------+
```

The next-to-last field, value, is quite straightforward—it contains the gathered value. The clock field contains the timestamp in Unix time—the number of seconds since the so-called Unix epoch, 00:00:00 UTC on January 1, 1970. The ns column contains nanoseconds inside that particular second.

> An easy way to convert the Unix timestamp to a human-readable form that does not require an Internet connection is using the GNU date command: `date -d@<timestamp>`. For example, `date -d@1234567890` will return `Sat Feb 14 01:31:30 EET 2009.`

The first field, itemid, is the most mysterious one. How can we determine which ID corresponds to which item? Again, the easiest way is to use the frontend. You should still have the item properties page open in your browser, so take a look at the address bar. Along with other variables, you'll see part of the string that reads like `itemid=23668`. Great, so we already have the itemid value on hand. Let's try to grab some values for this item from the database:

```
```sql
mysql> select * from history where itemid=23668 limit 3;
```
```

Use the itemid value that you obtained from the page URL:

```text
+--------+------------+--------+----------+
| itemid | clock      | value  | ns       |
+--------+------------+--------+----------+
|  23668 | 1430700808 | 0.0000 | 644043321 |
|  23668 | 1430700838 | 0.0000 | 651484815 |
|  23668 | 1430700868 | 0.0000 | 657907318 |
+--------+------------+--------+----------+
```

The resulting set contains only values from that item, as evidenced by the itemid field in the output.