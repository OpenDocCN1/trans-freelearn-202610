4. Now let’s try to insert another record with the same primary key. If we perform a standard insert statement, as follows, we can see that PostgreSQL returns an error because we are trying to insert a record that already exists:

```
forumdb=>insert into j_posts_tags (post_pk ,tag_pk) values (2,1);
ERROR:  duplicate key value violates unique constraint "j_posts_tags_pkey"
DETAIL:  Key (tag_pk, post_pk)=(1, 2) already exists.
```

5. Let’s now try using the ON CONFLICT DO NOTHING option:

```
forumdb=> insert into j_posts_tags (post_pk ,tag_pk) values (2,1) ON CONFLICT DO NOTHING;
INSERT 0 0

forumdb=> select * from j_posts_tags ;
tag_pk | post_pk
--------+---------
     2 |       3
     1 |       1
     1 |       2
(3 rows)
```

In this case, PostgreSQL doesn’t return an error; instead, it simply does nothing.

6. Now let’s try the DO UPDATE set option. This option realizes the UPSERT statement, as in the following example:

```
forumdb=> insert into j_posts_tags (post_pk ,tag_pk) values (2,1) ON CONFLICT (tag_pk,post_pk) DO UPDATE set tag_pk=excluded.tag_pk+1;
INSERT 0 1

forumdb=> select * from j_posts_tags ;
tag_pk | post_pk
--------+---------
     2 |       3
     1 |       1
     2 |       2
(3 rows)
```