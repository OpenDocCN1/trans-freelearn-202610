![](bbox=[0.12, 0.13, 0.2, 0.2])

Do not hesitate to inspect/1 an Ecto changeset, it will show you really useful information, such as the data you're manipulating, the changes that will be applied to it, eventual validation errors, and the action that should be performed when the changeset is actually persisted in the database. The field that you probably will use the most is valid?, which lets you to know whether the changeset is good to be persisted or not.

We will now see how we can create a new Media schema to represent the media of our users.

# Media

The changeset for the initial version of a Media schema will be created by the create_initial_changeset/4 function exposed by the Storage.Media module.

Given that the database does not know what KSUIDs are, we generate a unique identifier at this stage. It will also be used to generate a storage_key, that is the actual name of the file stored on the cloud storage:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/media.ex
defmodule ElixirDrip.Storage.Media do
  use Ecto.Schema

  alias __MODULE__
  alias Ecto.Changeset
  # ...
  def create_initial_changeset(user_id, file_name, full_path,
    file_size) do
    id = Ksuid.generate()

    attrs = %{
      id: id,
      user_id: user_id,
      storage_key: generate_storage_key(id, file_name),
      encryption_key: Encryption.generate_key(),
      file_name: file_name,
      full_path: full_path,
      file_size: file_size,
    }

    %Media{}
    |> Changeset.cast(attrs, cast_attrs())
    |> Changeset.validate_required(required_attrs())
    |> validate_field(:full_path)
```