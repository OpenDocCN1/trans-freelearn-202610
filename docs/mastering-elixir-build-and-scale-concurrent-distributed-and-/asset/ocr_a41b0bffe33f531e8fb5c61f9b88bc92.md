```
%{email: "jose@hey.ho", id: "13KvtrMQ9uRB8MqkJpKSSQZxnBa", username:
"jose"},
%{email: "xp@to.foo", id: "13ix90kpmblgJF4Fu1762gqb6m2", username:
"xpto"},
%{email: "daniel@right.here", id: "13ucVBdFw40QqbYjwm65jEm17P9", username: "daniel"},
%{email: "andre@somewhere.com", id: "13ucZ46LTdsfvezCPviNJ96gYqu", username: "andre"}
]
```

Notice how we are using the Query.API.map/2 function for Ecto to select the results into a map with only the id, username, and email fields, instead of defining the map manually by %{id: u.id, ...}. We could have defined the query as from User, select: [:id, :username, :email], but this would get us a list of User schemas, each with only those three fields filled.

Since we are storing each Media with the ID of the user who uploaded it (the media.user_id field), we can count the number of files and the disk usage per user just by looking at this schema:

```
iex> user_stats = from m in Media,
...> group_by: m.user_id,
...> select: %{
...> media_count: count(m.id),
...> disk_usage: sum(m.file_size),
...> user_id: m.user_id
...> }
```

```
#Ecto.Query<from m in ElixirDrip.Storage.Media,
group_by: [m.user_id],
select: %{media_count: count(m.id), disk_usage: sum(m.file_size), user_id: m.user_id}>
```

```
iex> Repo.all(user_stats)
```

```
10:42:26.430 [debug] QUERY OK source="storage_media" db=18.8ms decode=8.4ms queue=0.3ms
SELECT count(s0."id"), sum(s0."file_size"), s0."user_id" FROM
"storage_media" AS s0 GROUP BY s0."user_id" []
[
  %{disk_usage: 25, media_count: 1, user_id:
"13ix90kpmblgJF4Fu1762gqb6m2"},
  %{disk_usage: 161, media_count: 7, user_id:
"13ucZ46LTdsfvezCPviNJ96gYqu"},
]
```