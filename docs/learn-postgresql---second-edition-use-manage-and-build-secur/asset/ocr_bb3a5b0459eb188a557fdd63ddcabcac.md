```
forumdb=> alter table new_b_tags add constraint new_b_tags_pk primary key (pk);
ALTER TABLE
forumdb=> \d new_b_tags
       Table "forum.new_b_tags"

 Column |  Type   | Collation | Nullable | Default
--------+---------+-----------+----------+--------
 pk     | integer |           | not null |
 tag    | text    |           |          |
 parent | integer |           |          |
Indexes:
    "new_b_tags_pk" PRIMARY KEY, btree (pk)
```

Step 2 is now complete, and we have everything we need to start our complete example.

# Managing rules on INSERT, DELETE, and UPDATE events

The goal we want to achieve is shown in the following figure:

![](bbox=[0.17, 0.46, 0.83, 0.79])

Figure 8.1: Managing rules

We want all tags starting with the letter a to be stored in the new_tags table and also copied to the new_a_tags table, and we want the same for tags that begin with the letter b.