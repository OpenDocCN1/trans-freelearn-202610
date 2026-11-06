```
files: #Ecto.Association.NotLoaded<association :files is not loaded>,
id: "13KvtrMQ9uRB8MqkJpKSSQZxnBa"
}
```

If you want to access their files, you need to explicitly call the Repo.preload/2 callback with the schema and the field to load:

```
iex> Repo.preload(owner, :files)
11:18:06.335 [debug] QUERY OK source="storage_media" db=4.1ms decode=0.1ms
SELECT s0."id", s0."user_id", s0."file_name", s0."full_path",
s0."file_size", s0."metadata", s0."encryption_key", s0."storage_key",
s0."uploaded_at", s0."inserted_at", s0."updated_at", u1."id" FROM
"storage_media" AS s0 INNER JOIN "users" AS u1 ON u1."id" = ANY($1) INNER
JOIN "media_owners" AS m2 ON m2."user_id" = u1."id" WHERE (m2."media_id" =
s0."id") ORDER BY u1."id" [["13KvtrMQ9uRB8MqkJpKSSQZxnBa"]]

%ElixirDrip.Storage.Owner{
__meta__: #Ecto.Schema.Metadata<:loaded, "users">,
email: "jose@hey.ho",
files: [
%ElixirDrip.Storage.Media{ ... },
],
id: "13KvtrMQ9uRB8MqkJpKSSQZxnBa"
}
```

Arguably, explicitness is always good. However, this approach implies an additional query to retrieve the files (notice the additional SELECT statement after we called the Repo.preload/2 callback).

We can also use the Query.preload/3 macro when composing an Ecto query. Hence, the previous query could be rewritten as:

```
iex> query = from o in Owner,
...> where: o.id == "13KvtrMQ9uRB8MqkJpKSSQZxnBa",
...> preload: [:files]
#Ecto.Query<from o in ElixirDrip.Storage.Owner,
where: o.id == "13KvtrMQ9uRB8MqkJpKSSQZxnBa",
preload: [:files]>

iex> Repo.one(query)
14:23:22.140 [debug] QUERY OK source="users" db=1.5ms
# ...
14:23:22.143 [debug] QUERY OK source="storage_media" db=2.1ms decode=0.1ms
queue=0.2ms
# ...

%ElixirDrip.Storage.Owner{
```