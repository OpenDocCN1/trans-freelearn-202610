
If we now try to create a new user with the same "jose" username, we'll get the expected error tuple with the invalid changeset:

```
iex> Accounts.create_user(%{username: "jose", email: "abc@web.com", password: "secur3pwd"})
```

```
08:15:18.585 [debug] QUERY ERROR db=2.6ms
INSERT INTO "users" ("email","hashed_password","username","id","inserted_at","updated_at") VALUES ($1,$2,$3,$4,$5,$6) ["abc@web.com", "$2b$12$natqyxpfEjE2zYxbroZf4h.xXUOPclczH1lGxfOxapyi6BTF2uqRBm", "jose", "13jOqf9qgZePNpSRNOChyPfHfp8", {{2018, 4, 26}, {8, 15, 18, 582405}}, {{2018, 4, 26}, {8, 15, 18, 582420}}]
```

```
{:error,
#Ecto.Changeset<
  action: :insert,
  changes: %{
    email: "abc@web.com",
    hashed_password: "$2b$12$natqyxpfEjE2zYxbroZf4h.xXUOPclczH1lGxfOxapyi6BTF2uqRBm",
    password: "secur3pwd",
    username: "jose"
  },
  errors: [username: {"has already been taken", []}],
  data: #ElixirDrip.Accounts.User<>, 
  valid?: false
>}
```

Notice that, since we are relying on a database unique index, we only get the expected error when we actually hit the database with the Repo.insert/2 (or Repo.update/2) function.

Let's now use the same approach to ensure that each media can only be shared once with the same user. We will also create a database unique index, but this time applied to the two foreign key fields on the media_owners table, media_id and user_id. If someone tries to share a file with a user and there is already a MediaOwners entry for that media and user ID pair, the operation should not be allowed:

```
$ cat apps/elixir_drip/priv/repo/migrations/20180408161233_unique_shares_constraint.exs
defmodule ElixirDrip.Repo.Migrations.UniqueSharesConstraint do
  use Ecto.Migration

  def change do
    create unique_index(:media_owners, [:media_id, :user_id], name: :single_share_index)
  end
end
```

On this last migration, besides indicating the two media_id and user_id columns, we are also explicitly naming this unique constraint to :single_share_index. All constraints have names, so if we don't set it, Ecto will generate a name based on the table and field names for us. The default name in this case would be :media_owners_media_id_user_id_index, too verbose in my view.

We defined a constraint name because, as you can see here, we need to indicate it when calling Changeset.unique_constraint/3 on the Storage.share/3 function:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/storage.ex
defmodule ElixirDrip.Storage do
  # ...
  def share(user_id, media_id, allowed_user_id) do
    with {:ok, :creator} <- is_creator?(user_id, media_id) do
      %MediaOwners{}
      |> Changeset.cast(%{user_id: allowed_user_id, media_id: media_id}, [:user_id, :media_id])
      |> Changeset.unique_constraint(:existing_share, name: :single_share_index)
      |> Changeset.foreign_key_constraint(:user_id)
      |> Changeset.foreign_key_constraint(:media_id)
      |> Repo.insert()
    else
      error -> error
    end
  end
end
```

Let's now see what happens when we try to share a file again with the same user (xpto_user already had access to the given file):

```
iex> Storage.share(jose_user.id, file.id, xpto_user.id)
09:10:17.572 [debug] QUERY OK source="storage_media" db=2.5ms queue=0.1ms
SELECT s0."id", s0."user_id", s0."file_name", s0."full_path", s0."file_size", s0."metadata", s0."encryption_key", s0."storage_key", s0."uploaded_at", s0."inserted_at", s0."updated_at" FROM "storage_media" AS s0 WHERE (s0."id" = $1) AND (s0."user_id" = $2)
["13Kvv5GQey9oPFWEAqqSFECYGag", "13KvtrMQ9uRB8MqkJpKSSQZxnBa"]

09:10:17.572 [debug] QUERY OK db=0.3ms queue=0.1ms
begin []

09:10:17.575 [debug] QUERY ERROR db=2.0ms
INSERT INTO "media_owners" ("media_id","user_id","inserted_at","updated_at") VALUES ($1,$2,$3,$4)
RETURNING "id" ["13Kvv5GQey9oPFWEAqqSFECYGag",
"13ix90kpmblgJF4Fu1762gqb6m2", {{2018, 4, 26}, {9, 10, 17, 572987}},
{{2018, 4, 26}, {9, 10, 17, 573007}}]

09:10:17.575 [debug] QUERY OK db=0.2ms
rollback []

{:error,
 #Ecto.Changeset<
   action: :insert,
   changes: %{
     media_id: "13Kvv5GQey9oPFWEAqqSFECYGag",
     user_id: "13ix90kpmblgJF4Fu1762gqb6m2"
   },
   errors: [existing_share: {"has already been taken", []}],
   data: #ElixirDrip.Storage.MediaOwners<>, 
   valid?: false
 >}
```

As expected, the database rollbacks the insert operation and we get an error saying the `existing_share` "has already been taken". This message is the default error message; if you want to change it, you can pass the `:message` option when we call the `unique_constraint/3` function.

Notice how we also use the `Changeset.foreign_key_constraint/3` function to achieve the same error-handling behavior, but this time for the two foreign keys we have in place. If we didn't have these calls, Ecto would raise an error if a non-existing ID was used as one of the foreign keys:

```
iex> Storage.share(jose_owner.id, file.id, "13Kvv5GQey9oPFWEAqqINVALID!")
# ...
{:error,
 #Ecto.Changeset<
   action: :insert,
   changes: %{
     media_id: "13Kvv86zvDHF6xJSZMNsfBZrN5o",
     user_id: "13Kvv5GQey9oPFWEAqqINVALID!"
   },
   errors: [user_id: {"does not exist", []}],
   data: #ElixirDrip.Storage.MediaOwners<>, 
   valid?: false
 >}
```

Besides sharing, we also allow our users to delete their ElixirDrip media (as long as it was uploaded by them). These files might have been shared with other users in the meantime, so we also need to take care of the lingering media shares on the `MediaOwners` schema.

To elegantly solve this, we'll ask the database to do the heavy-lifting for us once more. We will recreate the media_id foreign key we have on the media_owners table, so that, when a Media entry is deleted from the database, all the existing media shares on MediaOwners referencing the deleted Media will also be deleted. To achieve this cascading behavior, we just need to create the foreign key again with the :on_delete option set to :delete_all:

```
$ cat apps/elixir_drip/priv/repo/migrations/20180409214938_change_media_owners_on_delete_media.exs
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
Repo.preload(:files)
# ...

iex> xpto_user_with_files.files |> Enum.count
2

iex> Storage.delete(jose_user.id, file.id)
08:55:04.018 [debug] QUERY OK db=3.3ms
DELETE FROM "storage_media" WHERE "id" = $1 ["13Kvv5GQey9oPFWEAqqSFECYGag"]
# ...

{:ok,
  %ElixirDrip.Storage.Media{
    __meta__: #Ecto.Schema.Metadata<:deleted, "storage_media">,
    id: "13Kvv5GQey9oPFWEAqqSFECYGag",
    # ...
  }}

iex> xpto_user_with_files = xpto_user_with_files |> Repo.preload(:files,
force: true)
# ...

iex> xpto_user_with_files.files |> Enum.count
1
```

In this section, we saw how Ecto allows us to easily put to good use the powerful tools provided by the database. In the next section, we'll continue to interact with the database, ranging from writing queries in Elixir syntax to using plain SQL when needed.

## Queries

At the beginning of this chapter, we ran a SQL query with Repo.query/3 as soon as we had a database connection, to verify that our connection was working as expected.
Ultimately, Ecto is all about flexibility, so it lets you work with raw SQL as easily as you work with its query API.

![](img/c636f4cfb07f25f1db535995e50706ed_325_0.png)

We need to import the Ecto.Query module so we can write Ecto queries without the need to always prefix the needed macros (for example, where/3 and group_by/3).
