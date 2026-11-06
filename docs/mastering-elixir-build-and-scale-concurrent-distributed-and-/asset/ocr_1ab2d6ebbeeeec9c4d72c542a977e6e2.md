```
id: "13Kvvb4o0gzSYT2HcCGAhAkEZGt",
  name: "README.md",
  size: 14
}
],
  folders: [ %{files: 1, name: "second", size: 19} ]
}

iex> Storage.media_by_folder("13KvtrMQ9uRB8MqkJpKSSQZxnBa",
  "$/nowhere/to/be/found")
%{files: [], folders: []}
```

With the previous data structure, the ElixirDripWeb umbrella app will be able to render a pretty list of files and folders for any path.

We will compose the previous function step by step, reusing any partial queries that we may have along the way. We will start by implementing the Storage.user_media_query/1 function, which returns all the media accessible by a given user:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/storage.ex
defmodule ElixirDrip.Storage do
  import Ecto.Query
  # ...
  defp user_media_query(user_id) do
    from media_owner in MediaOwners,
      join: media in Media,
      on: media_owner.media_id == media.id,
      where: media_owner.user_id == ^user_id,
      select: media
  end
end
```

This query joins the MediaOwners and Media schemas and returns all the media for that user_id. To use external values on the query, such as our user_id, we need to prepend them with ^. Also notice how we are able to select only the media fields by indicating the select: media clause. If we omitted the select clause, the query would return both schemas with all the fields.

The previous query is used in different Storage functions, including in the following user_media_on_folder/2 function:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/storage.ex
defmodule ElixirDrip.Storage do
  import Ecto.Query
  import ElixirDrip.Storage.Macros
```