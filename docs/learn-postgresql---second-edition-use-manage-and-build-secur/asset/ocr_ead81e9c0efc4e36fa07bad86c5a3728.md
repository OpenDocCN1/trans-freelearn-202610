```
left join j_posts_tags jpt on p.pk=jpt.post_pk
left join tags t on jpt.tag_pk=t.pk
group by 1,2 order by 1) Q;
INSERT 0 3
```

Now, the post_json table has the following records:

```
forumdb=> select jsonb_pretty(jsondata) from post_json;
-[ RECORD 1 ]+-------------------------
jsonb_pretty | {
             |   "pk": 5,
             |   "tag": "Operating Systems,Database",
             |   "title": "Indexing PostgreSQL"
             | }
-[ RECORD 2 ]+-------------------------
jsonb_pretty | {
             |   "pk": 6,
             |   "tag": "Database,Operating Systems",
             |   "title": "Indexing Mysql"
             | }
-[ RECORD 3 ]+-------------------------
jsonb_pretty | {
             |   "pk": 7,
             |   "tag": "Database",
             |   "title": "A view of  Data types in C++"
             | }
```

If we wanted to search for all data that has tag = "Database", we could use the @> jsonb operator. This operator checks whether the left JSON value contains the right JSON path/value entries at the top level; the following query makes this search possible:

```
forumdb=> select jsonb_pretty(jsondata) from post_json where jsondata @> '{"tag":"Database"}';

-[ RECORD 1 ]+-------------------------
jsonb_pretty | {
             |   "pk": 7,
             |   "tag": "Database",
```