```
Table "public.new_tags"
Column | Type      | Collation | Nullable | Default
--------+-----------+-----------+----------+--------
pk     | integer   |           | not null |
tag    | text      |           |          |
parent | integer   |           |          |
Indexes:
  "new_tags_pk" PRIMARY KEY, btree (pk)
```

With this, step 1 is complete.

# Creating two tables

Similar to what we just did, let’s create new_a_tags and new_b_tags tables. For the new_a_tags table, we will have the following:

```
forumdb=> create table new_a_tags as select * from tags limit 0;
SELECT 0
forumdb=> alter table new_a_tags alter pk set not null ;
ALTER TABLE
forumdb=> alter table new_a_tags add constraint new_a_tags_pk primary key (pk);
ALTER TABLE
forumdb=> \d new_a_tags
               Table "forum.new_a_tags"
 Column |  Type   | Collation | Nullable | Default
--------+---------+-----------+----------+---------
pk     | integer |           | not null |
tag    | text    |           |          |
parent | integer |           |          |
Indexes:
  "new_a_tags_pk" PRIMARY KEY, btree (pk)
```

In the same way, we will create the new_b_tags table:

```
forumdb=> create table new_b_tags as select * from tags limit 0;
SELECT 0
forumdb=> alter table new_b_tags alter pk set not null ;
ALTER TABLE
```