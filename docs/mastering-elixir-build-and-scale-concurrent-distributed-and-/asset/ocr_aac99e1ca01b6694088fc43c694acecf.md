The following query orders the results in descending order, by number of uploaded files. As you can see, it only adds the order_by clause to the schemaless_users_media query:

```
iex> schemaless_ordered_users_media = from [u,m] in schemaless_users_media,
...> order_by: [desc: 1]
```

```
#Ecto.Query<from u in "users", left_join: s in "storage_media",
 on: u.id == s.user_id, group_by: [u.id], order_by: [desc: 1],
 select: %{media_count: count(s.id), disk_usage: sum(s.file_size),
 username: u.username}>
```

```
iex> Repo.all(schemaless_ordered_users_media)
```

```
13:54:24.179 [debug] QUERY OK source="users" db=1.5ms queue=0.1ms
SELECT count(s1."id"), sum(s1."file_size"), u0."username" FROM "users" AS u0 LEFT OUTER JOIN "storage_media" AS s1 ON u0."id" = s1."user_id" GROUP BY u0."id" ORDER BY 1 DESC []
```

```
[
 %{disk_usage: 103, media_count: 8, username: "jose"},
 %{disk_usage: 161, media_count: 7, username: "andre"},
 %{disk_usage: 25, media_count: 1, username: "xpto"},
 %{disk_usage: nil, media_count: 0, username: "daniel"}
]
```

Ecto orders the results in descending-number-of-files order, because we passed the [desc: 1] keyword list to the order_by/2 macro. The 1 indicates we want the results ordered by the first query column—in our case, the database column with the count result.

![](bbox=[0.12, 0.62, 0.2, 0.75])

Despite the media_count entry appearing as the second entry of every map in the query outcome (that is, [%{disk_usage: 103, media_count: 8, username: "jose"}, ...]), the file tally appears on the first column of the query (that is, SELECT count(s1."id"), sum(s1."file_size"), u0."username" ...), hence the desc: 1 order criterion pointing to the first column of the results.

Despite ordering as expected, indicating the column number isn't intuitive, and relies on the column order. Let's use the previous query as a subquery, and see how can we dynamically define both the sorting column and direction, instead of relying on an arbitrary integer:

```
iex> sort_order = :desc
:desc
```

```
iex> dynamic_order_schemaless_users_media = from e in subquery(schemaless_users_media),
...> order_by: [^sort_order, e.media_count}]
```