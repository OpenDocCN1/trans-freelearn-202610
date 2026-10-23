
Column | Type | Collation | Nullable | Default
-------------------------------+-------------------+------------+----------+----------------
pk | integer | | not null | generated always as identity
title | text | | | 
content | text | | | 
author | integer | | not null |
category | integer | | not null |
reply_to | integer | | | 
created_on | timestamp with time zone | | | CURRENT_TIMESTAMP
last_edited_on | timestamp with time zone | | | CURRENT_TIMESTAMP
editable | boolean | | | true
likes | integer | | | 0
Indexes:
  "posts_pkey" PRIMARY KEY, btree (pk)
  "idx_author_created_on" btree (author, created_on)
  "idx_post_category" btree (category)
  "idx_post_created_on" hash (created_on)
...

As you can see from the preceding snippet of code, the command shows all the available indexes with their method (for example, btree) and a list of the columns the index is built on top of.

The pg_index special catalog contains information about the indexes and their main attributes, and so it can be queried to get the very information (and more) that is provided by psql. In particular, since an index is registered into pg_class with the special relkind value of i, we can join pg_class and pg_index to get detailed information in a statement, as follows:

```
forumdb=> SELECT relname, relpages, reltuples,
    i.indisunique, i.indisclustered, i.indisvalid,
    pg_catalog.pg_get_indexdef(i.indexrelid, 0, true)
FROM pg_class c JOIN pg_index i ON c.oid = i.indrelid
WHERE c.relname = 'posts';
```

```
-[ RECORD 1 ]---+-------------------------------------------------------------------------------
-----------------
relname       | posts
relpages      | 21
reltuples     | 1004
indisunique   | t
indisclustered | f
 indisvalid    | t
pg_get_indexdef | CREATE UNIQUE INDEX posts_pkey ON posts USING btree (pk)
-[ RECORD 2 ]---+-------------------------------------------------------------------------------
-----------------
relname       | posts
relpages      | 21
reltuples     | 1004
indisunique   | f
indisclustered | f
 indisvalid    | t
pg_get_indexdef | CREATE INDEX idx_post_category ON posts USING btree (category)
-[ RECORD 3 ]---+-------------------------------------------------------------------------------
-----------------
relname       | posts
relpages      | 21
reltuples     | 1004
indisunique   | f
indisclustered | f
 indisvalid    | t
pg_get_indexdef | CREATE INDEX idx_author_created_on ON posts USING btree (author, created_on)
-[ RECORD 4 ]---+-------------------------------------------------------------------------------
-----------------
relname       | posts
relpages      | 21
reltuples     | 1004
indisunique   | f
indisclustered | f
```

```
indisvalid | t
pg_get_indexdef | CREATE INDEX idx_post_created_on ON posts USING hash (created_on)
```

The indisunique column is set to true if the index has been created with the UNIQUE clause, as it happens for the primary key index. indisvalid is a boolean value that indicates whether the index is usable or not (as you will see later on, you can decide to disable an index for any reason). Since you can cluster a table against an index – that is, you can sort the table depending on a specific index – indisclustered indicates whether the table is clustered against the specific index.

The pg_get_indexdef() special function provides a textual representation of the CREATE INDEX statement used to produce every index and can be very useful to decode and learn how to build complex indexes.

Therefore, either using the psql \d command or querying pg_index, you can get details about existing indexes and their status.

## Dropping an index

In order to discard an index, you need to use the DROP INDEX statement, which looks like the following:

```
DROP INDEX [ CONCURRENTLY ] [ IF EXISTS ] name [, ...] [ CASCADE | RESTRICT ]
```

The statement accepts the name of the index and can drop more than one index at the same time if you specify multiple names on the same statement.

The CONCURRENTLY clause prevents the command from acquiring an exclusive lock on the underlying table, preventing other queries from accessing the table until the index has been dropped. Note, this clause cannot always be used; for example, it cannot be used within an explicit transaction.

The CASCADE option drops the index and all other objects that depend on the index (e.g., a table constraint), while the RESTRICT option is its counterpart and prevents the index from being dropped if any object still insists on the index. The RESTRICT clause is the default.

Lastly, the IF EXISTS option allows the command to gracefully abort if the index has already been dropped.

## Invalidating an index

It is possible to invalidate an index, which is a way to tell PostgreSQL to not consider that index at all without dropping the index and building it again. This can be useful in situations where you are studying your cluster’s behavior and want to force the optimizer to choose another path to access the data that does not include a specific index, or it can be necessary when there is a problem with an index.

In order to invalidate an index, you have to directly manipulate the pg_index system catalog to set the indisvalid attribute to false. For example, in order to suspend the usage of the idx_author_created_on index, you have to do an update against pg_index, as follows:

```
forumdb=# UPDATE pg_index SET indisvalid = false
    WHERE indexrelid = ( SELECT oid FROM pg_class
        WHERE relkind = 'i'
        AND relname = 'idx_author_created_on' );

UPDATE 1

forumdb=# \d posts
...
Indexes:
  "posts_pkey" PRIMARY KEY, btree (pk)
  "idx_author_created_on" btree (author, created_on) INVALID
  "idx_post_category" btree (category)
  "idx_post_created_on" hash (created_on)
...
```

> You need to invalidate an index as an administrator user, even if you are the user that created the index. This is due to the fact that you need to manipulate the system catalog, which is an activity restricted to administrator users only.

As you can see, the index is then marked as INVALID to indicate that PostgreSQL will not ever try to consider it for its execution plans. You can, of course, reset the index to its original status, making the same update as the preceding and setting the indisvalid column to a true value.

- It will not execute the statement you are asking the plan for, at least unless you explicitly ask for its execution. Therefore, the EXPLAIN execution is fast and pretty much constant each time.
- It will present you with all the execution nodes that the executor will use to provide you with the dataset.

Let’s see an example of EXPLAIN in action to better understand. Imagine we need to understand the execution plan of the SELECT * FROM categories statement. In this case, you need to prefix the statement with the EXPLAIN command, as follows:

```
forumdb=> EXPLAIN SELECT * FROM categories;

            QUERY PLAN

------------------------------------------------------------
Seq Scan on categories  (cost=0.00..1.05 rows=5 width=68)
(1 row)
```

As you can see, the output of EXPLAIN reports the query plan. There is a single execution node, of the Seq Scan type, followed by the table against which the node is executed (on categories). In the output of the EXPLAIN command, you will find all the types of execution nodes already discussed in the previous sections.

For every node, EXPLAIN will report some more information between parentheses: the cost, the number of rows, and the width. The cost is the amount of effort required to execute the node, and is always expressed as a “startup cost” and a “final cost.” The startup cost is how much work PostgreSQL has to do before it begins executing the node; in the preceding example, the cost is 0, meaning the execution of the node can begin immediately. The final cost is how much effort PostgreSQL has to do to provide the last bit of the dataset – that is, to complete the execution of the node.

The rows field indicates how many tuples the node is expected to provide in the final dataset, and is a pure estimation. Being an estimation, the value could be wrong and you have to keep in mind that it will never be zero: when PostgreSQL estimates a very low number of tuples, it always provides 1 as the number of rows.

Lastly, the width field indicates how many bits every tuple will occupy, as an average. Essentially, this information is used to estimate the network traffic that the query will produce: in the preceding example, it is possible to estimate 68 bytes per tuple.
