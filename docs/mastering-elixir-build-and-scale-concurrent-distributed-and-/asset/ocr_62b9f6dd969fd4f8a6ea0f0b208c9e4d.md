If we consider that Ecto relies on available SQL functions to calculate the aggregate, this makes total sense. After all, in standard SQL, a query with more than one GROUP BY clause isn't valid. As such, we will wrap the schemaless_users_media in a subquery/2, thus fixing the Repo.aggregate/3 call. By using subquery, the outermost query ends up without any GROUP BY clause, allowing Repo.aggregate/3 to add its own GROUP BY clause:

```
iex> subquery(schemaless_users_media) |> Repo.aggregate(:avg, :media_count)
15:40:50.854 [debug] QUERY OK db=4.5ms queue=0.1ms
SELECT avg(s0."media_count") FROM (SELECT count(s1."id") AS "media_count", sum(s1."file_size") AS "disk_usage", u0."username" AS "username" FROM "users" AS u0 LEFT OUTER JOIN "storage_media" AS s1 ON u0."id" = s1."user_id" GROUP BY u0."id") AS s0 []
#Decimal<4.0000000000000000>
```

We now know that each user stores an average of four files in ElixirDrip.

As we saw in this section, Ecto provides us with some terrific query tools that give us a lot of power and flexibility. You can query your data by relying on the schemas you've defined, or you can simply interact with the database tables and hand-pick the fields you want to retrieve. If Ecto doesn't provide an abstraction for a specific SQL function you need, you can directly invoke the SQL function from your queries, no questions asked.

It is worth noting that we aren't tied at all to Ecto schemas. For queries that calculate aggregates and/or span many tables, going schemaless is often the simplest solution, and lets you avoid the pain of trying to adapt your query output to the existing schemas you may have.

# Summary

In this chapter, we not only understood what Ecto is, but also applied its four main concepts–Repository, Schemas, Changesets, and Queries–to our ElixirDrip umbrella project:

+   - We configured the application repository so we could connect (with the help of the Ecto adapter and the database driver) to our PostgreSQL database
+   - We defined the schemas that represent our database tables
+   - We learned how to use changesets to validate the changes made to information stored in the database