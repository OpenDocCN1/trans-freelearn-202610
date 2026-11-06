On the Storage.share/3 function, we start by ensuring the user sharing the media is the one who uploaded it in the first place (the is_creator?/2 function will be analyzed later in this chapter). If the user is the media creator, we then proceed to create and insert a changeset associating media_id with allowed_user_id, allowing a new user to view and download the media in question. By creating a new entry in the MediaOwners join schema, we are letting one more user access the given media file.

If we also wanted to use the Changeset.put_assoc/3 approach here, we would have to first fetch a list with all the existing media owners, and then add the new allowed user to this list. Despite being a valid alternative, this approach would imply an additional database query to fetch the existing owners.

Some of the operations analyzed in this section use underlying database constraints to ensure the consistency of our data. In the next section, we'll understand how Ecto leverages database constraints and allows us to easily work with them.

# Constraints

When we analyzed the User.create_changeset/2 function, we didn't have any validation in place to check whether the username we are storing in the database already exists or not. For a system like ours, this could spell trouble. As such, let's create a unique index on the users table for the username column:

```
$ cat apps/elixir_drip/priv/repo/migrations/
20180320101722_unique_users_constraint.exs
defmodule ElixirDrip.Repo.Migrations.UniqueUsersConstraint do
  use Ecto.Migration

  def change do
    create unique_index(:users, [:username])
  end
end
```

After we apply this migration with mix ecto.migrate, the database will start to enforce username uniqueness on any operation on the users table. Let's see what happens if we try to update the xpto_user username to an already existing username:

```
iex> xpto_user
%ElixirDrip.Accounts.User{ ... }

iex> update_op = Changeset.cast(xpto_user, %{username: "jose"},
  [:username])
#Ecto.Changeset<
```