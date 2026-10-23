```
ON posts( category );
CREATE INDEX
```

The preceding code will create an index named idx_post_category on the posts table, using the single-column category and the default index type (B-Tree).

The following does something similar, creating a multi-column index:

```
forumdb=> CREATE INDEX idx_author_created_on
    ON posts( author, created_on );
CREATE INDEX
```

It is important to note that, when creating multi-column indexes, you should always place the most selective columns first. PostgreSQL will consider a multi-column index from the first column onward, so if the first columns are the most selective, the index access method will be the cheapest. In the preceding example, assuming we want to search for a combination of authors and dates, we could expect many authors to publish on a specific day, so the date (the created_on column) is not going to be very selective, at least not as selective as the specific author; it is for that reason that we pushed the created_on column to the right in the column list.

If we would like to create a hash index, we could do something such as the following:

```
forumdb=> CREATE INDEX idx_post_created_on
    ON posts USING hash ( created_on );
CREATE INDEX
```

Of course, such an index will be useful only for equality comparison, so a query such as the following will never use the preceding index:

```
SELECT * FROM posts WHERE created_on < CURRENT_DATE;
```

But a query like the following could use the hash index:

```
SELECT * FROM posts WHERE created_on = CURRENT_DATE;
```

This is because we are asking for an equality comparison.

# Inspecting indexes

Indexes are “attached” to their underlying tables, and so psql shows the defined indexes whenever you ask it to describe a table with the \d special command:

```
forumdb=> \d posts

Table "forum.posts"
```