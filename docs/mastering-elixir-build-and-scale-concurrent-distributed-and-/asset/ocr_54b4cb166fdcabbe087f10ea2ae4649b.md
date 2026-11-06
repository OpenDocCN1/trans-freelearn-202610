![](bbox=[0.1, 0.1, 0.9, 0.35])

In database parlance, we have a classical many-to-many relation. As such, we need an additional association table, since we already have the media and users database tables. Let's create the media_owners table with a migration:

```
$ cat apps/elixir_drip/priv/repo/migrations/20180408150211_create_media_owners.exs
defmodule ElixirDrip.Repo.Migrations.CreateMediaOwners do
  use Ecto.Migration

  def change do
    create table(:media_owners) do
      add :media_id, references(:storage_media, type: :string)
      add :user_id, references(:users, type: :string)

      timestamps(type: :utc_datetime)
    end
  end
end
```

This migration creates the media_owners table with two string columns, media_id and user_id. By using the references/2 macro for both fields, they will be defined as foreign keys. Every value that we place on the media_id and user_id columns needs to exist on the id column of the storage_media and users tables, respectively. We didn't need to use the name option to indicate the primary key of the referenced tables because it defaults to :id, which is the name of our primary key fields on both tables.