We are passing the :primary_key option as false on the table/2 call, because we want to explicitly define it as a 27-character string field afterwards. The table/2 function will return a Table struct that represents the table that will be created, whereas the create/2 macro will receive the Table struct and the list of fields, and will create the corresponding table.

Let's run the migration and then check its status with the ecto.migrations Mix task:

```
$ mix ecto.migrate
18:16:18.548 [info] == Running
ElixirDrip.Repo.Migrations.Storage.Media.change/0 forward
18:16:18.548 [info] create table storage_media
18:16:18.572 [info] == Migrated in 0.0s

$ mix ecto.migrations

Repo: ElixirDrip.Repo

    Status Migration ID Migration Name
    ----------------------------------------
    up 20180112095312 create_media
```

As you can see, our create_media migration was correctly applied. We will now define the schemas and migrations that will materialize the relationship between our users and their media.

# Relationships

Our objective here is to implement the relationship between the Media and Owner schemas. In the beginning, a media file will only have a single owner: the user who uploaded the media into the system. This user can then share it with other users. As such, a media file may be owned by more than one user, and it will certainly be owned by at least one.

On the opposite side of this relationship, we have the media owners. A recently-created user won't have any media files, but eventually it will have dozens of files: