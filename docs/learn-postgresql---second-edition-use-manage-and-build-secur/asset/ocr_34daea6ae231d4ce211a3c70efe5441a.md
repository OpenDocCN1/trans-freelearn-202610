```
------+-----+--------------------------------+-----------------------------------
 561 |   1 | DATABASE                       | Database related discussions
 561 |   2 | UNIX                           | Unix and Linux discussions
 561 |   3 | PROGRAMMING LANGUAGES          | All about programming languages
(3 rows)
```

As you can see, all the tuples in the preceding table have been created by the very same transaction, number 561.

> PostgreSQL manages a few different hidden columns that you need to explicitly ask for when querying a table to be able to see them. In particular, every table has the xmin, xmax, cmin, and cmax hidden columns. Their use and aim will be explained later in this chapter.

Now that you know that every transaction is numbered and that such numbers are used to label tuples in every table, we can move forward and see the difference between implicit and explicit transactions.

# Comparing implicit and explicit transactions

Implicit transactions are those that you don’t ask for but that the system applies to your statements. In other words, it is PostgreSQL that decides when the transaction starts and when it ends (transaction boundaries), and the rule is simple: every single statement is executed in its own separate transaction.

In order to better understand this concept, let’s insert a few records into a table:

```
forumdb=> INSERT INTO tags( tag ) VALUES( 'linux' );
INSERT 0 1
forumdb=> INSERT INTO tags( tag ) VALUES( 'BSD' );
INSERT 0 1
forumdb=> INSERT INTO tags( tag ) VALUES( 'Java' );
INSERT 0 1
forumdb=> INSERT INTO tags( tag ) VALUES( 'Perl' );
INSERT 0 1
forumdb=> INSERT INTO tags( tag ) VALUES( 'Raku' );
INSERT 0 1
```