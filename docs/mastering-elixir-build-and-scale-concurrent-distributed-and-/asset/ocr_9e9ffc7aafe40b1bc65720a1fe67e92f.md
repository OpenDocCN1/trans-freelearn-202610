To elegantly solve this, we'll ask the database to do the heavy-lifting for us once more. We will recreate the media_id foreign key we have on the media_owners table, so that, when a Media entry is deleted from the database, all the existing media shares on MediaOwners referencing the deleted Media will also be deleted. To achieve this cascading behavior, we just need to create the foreign key again with the :on_delete option set to :delete_all:

```
$ cat apps/elixir_drip/priv/repo/migrations/
20180409214938_change_media_owners_on_delete_media.exs
defmodule ElixirDrip.Repo.Migrations.ChangeMediaOwnersOnDeleteMedia do
  use Ecto.Migration

  def change do
    drop constraint(:media_owners, :media_owners_media_id_fkey)

    alter table(:media_owners) do
      modify :media_id, references(:storage_media, type: :string,
        on_delete: :delete_all)
    end
  end
end
```

The previous migration makes our Storage.delete/2 function as concise as possible, since the deletion of media shares will be automatically taken care of by the database. Since Repo.delete/2 expects a struct, we'll pass it a Media struct only containing the media ID to delete:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/storage.ex
defmodule ElixirDrip.Storage do
  # ...
  def delete(user_id, media_id) do
    with {:ok, :creator} <- is_creator?(user_id, media_id) do
      %Media{id: media_id}
      |> Repo.delete()
    else
      error -> error
    end
  end
end
```

Now if jose_user, the owner of the file we are using, deletes it, we will see that the xpto_user number of files is reduced by 1. In the following example, we are using the Repo.preload/3 callback to fetch the files associated with an owner. This is how we fetch, or preload, all associations of a given struct. Since we're passing the :force option to it, Ecto will try to fetch the associations again, even if they're already loaded on the struct:

```
iex> xpto_user_with_files = Storage.get_owner(xpto_user.id) |>
```