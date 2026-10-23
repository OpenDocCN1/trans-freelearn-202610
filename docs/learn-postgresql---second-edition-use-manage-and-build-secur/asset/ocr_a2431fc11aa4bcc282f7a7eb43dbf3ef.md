```
-----------------+---------------+----------
pk      | integer       | [...] |
ins_date | date          | [...] |
tag     | character varying(255) | [...] |
level   | integer       | [...] |

Partition key: RANGE (ins_date)
Indexes:
    "part_tags_pkey" PRIMARY KEY, btree (pk, ins_date)
    "part_tags_tag_idx" btree (tag)
Partitions: part_tags_date_01_2023 FOR VALUES FROM ('2023-01-01') TO ('2023-01-31'),
            part_tags_date_02_2023 FOR VALUES FROM ('2023-02-01') TO ('2023-02-28'),
            part_tags_date_03_2023 FOR VALUES FROM ('2023-03-01') TO ('2023-03-31'),
            part_tags_date_04_2023 FOR VALUES FROM ('2023-04-01') TO ('2023-04-30')
```

# Attaching an existing table to the parent table

To practice this we need a table called part_tags_already_exists present in our database and containing all the tags with an entry date prior to 2022-12-31. If you are using the Docker image, you can find this inside the forumdb database. Otherwise, make sure to create the table with the following structure:

```
forumdb=> \d part_tags_already_exists
                 Table "forum.part_tags_already_exists"
 Column |      Type      | Collation | Nullable | Default
--------+----------------+-----------+----------+--------
pk      | integer        |           | not null |
ins_date | date           |           | not null |
```