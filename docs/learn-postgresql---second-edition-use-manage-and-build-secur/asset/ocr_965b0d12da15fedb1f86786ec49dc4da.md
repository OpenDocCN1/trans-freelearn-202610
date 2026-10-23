```
Partition of: part_tags FOR VALUES IN (0)
Indexes:
  "part_tags_level_0_pkey" PRIMARY KEY, btree (pk, level)
  "part_tags_level_0_tag_idx" btree (tag)
```

5. Let’s now perform some INSERT operations:

```
forumdb=> insert into part_tags (tag,level) values ('Operating System',0);
INSERT 0 1
forumdb=> insert into part_tags (tag,level) values ('Linux',1);
INSERT 0 1
forumdb=> insert into part_tags (tag,level) values ('BSD Unix',1);
INSERT 0 1
forumdb=> insert into part_tags (tag,level) values ('DOS',1);
INSERT 0 1
forumdb=> insert into part_tags (tag,level) values ('Windows',2);
INSERT 0 1
```

6. Finally, let’s check whether everything is okay:

```
forumdb=> select * from part_tags;
pk | level |      tag
----+-------+-----------------
  1 |     0 | Operating System
  2 |     1 | Linux
  3 |     1 | BSD Unix
  4 |     1 | DOS
  5 |     2 | Windows
(5 rows)

forumdb=> select * from part_tags_level_0;
pk | level |      tag
----+-------+-----------------
  1 |     0 | Operating System
```