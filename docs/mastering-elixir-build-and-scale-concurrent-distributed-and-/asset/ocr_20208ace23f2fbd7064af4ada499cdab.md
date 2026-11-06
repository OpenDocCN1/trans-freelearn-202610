```
%ElixirDrip.Storage.Owner{
  __meta__: #Ecto.Schema.Metadata<:loaded, "users">,
  email: "jose@hey.ho",
  files: [
    %ElixirDrip.Storage.Media{
      file_name: "proper_name.txt",
      # ...
    },
  ]
}
```

In the next section, we will learn how can we validate before doing a move or rename operation if a file already exists.

# Queries with raw SQL

Both the move/3 and rename/3 functions rely on the private _move/4 function, which is able to change simultaneously the full path and filename of a media file:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/storage.ex
defmodule ElixirDrip.Storage do
  # ...
  def move(user_id, media_id, new_path),
    do: _move(user_id, media_id, new_path, nil)

  def rename(user_id, media_id, new_name),
    do: _move(user_id, media_id, nil, new_name)

  defp _move(user_id, media_id, new_path, new_name), do: ...
  # ...
end
```

This _move/4 function only allows the media file creator to change its folder or its filename. Then, it gets the media file, the new path, and filename, and checks whether a file on the new path exists with the new name. If everything went well, it creates the changeset, validates the new full_path and file_name fields, and updates the Media schema:

```
defp _move(user_id, media_id, new_path, new_name) do
  with {:ok, :creator} <- is_creator?(user_id, media_id),
    %Media{} = media <- get_media(media_id),
    new_path <- new_path(media, new_path),
    new_name <- new_name(media, new_name),
    {:ok, :nonexistent} <- media_already_exists?(user_id, new_path, new_name)
  do
    media
```