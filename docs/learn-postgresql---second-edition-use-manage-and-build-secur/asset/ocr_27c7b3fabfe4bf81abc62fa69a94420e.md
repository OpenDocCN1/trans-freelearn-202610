```
3 | Programming Languages | All about programming languages
(1 row)
```

The preceding query returns all records that have pk > 2.

Another condition that we can use with the SELECT statement is the like condition. Let’s take a look at this next.

# Using the like clause

Suppose we wanted to find all records that have a title field value starting with the string Prog.

To do this, we would have to use the like condition:

```
forumdb=> \x
Expanded display is on.
forumdb=> select * from categories where title like 'Prog%';
-[ RECORD 1 ]---------------------------------
pk          | 3
title       | Programming Languages
description | All about programming languages
```

As shown, the preceding query returns all records that have a title beginning with the string Prog. In a similar vein, if we wanted to find all records with titles ending with the word Languages, we would have to write the following:

```
forumdb=> select * from categories where title like '%Languages';
-[ RECORD 1 ]---------------------------------
pk          | 3
title       | Programming Languages
description | All about programming languages
```

The two kinds of searches can also be combined. For example, if we wanted to search all records that contain the partial string discuss, we would write the following:

```
forumdb=> \x
Expanded display is off
forumdb=> select * from categories where description like '%discuss%';
pk | title   | description
----+---------+----------------------------
```