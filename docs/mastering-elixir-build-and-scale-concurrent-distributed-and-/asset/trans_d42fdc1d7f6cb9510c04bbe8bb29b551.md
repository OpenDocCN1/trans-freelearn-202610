
## 媒体所有权

在之前的 Media 部分，我们在为用户保存媒体文件时使用 Storage.store/3 函数简单地插入了一个新的 Media 条目。然而，这只是一个部分解决方案，因为 Media 和 Owner 模式之间存在多对多关系。

我们此处的目标是在首次使用 Storage.store/3 函数存储文件时创建一个新的 MediaOwners 模式条目，从而将用户与上传的媒体关联起来：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/storage.ex
defmodule ElixirDrip.Storage do
  # ...
  def store(user_id, file_name, full_path, content) do
    file_size = byte_size(content)
    with %Owner{} = owner <- get_owner(user_id),
         %Changeset{} = changeset <- Media.create_initial_changeset
           (owner.id, file_name, full_path, file_size),
         %Changeset{} = changeset <- Changeset.put_assoc(changeset,
           :owners, [owner]),
         {:ok, %Media{} = media} <- Repo.insert(changeset)
    do
      # ... upload task enqueue

      {:ok, :upload_enqueued, media}
    else
      error -> error
    end
  end
  # ...
end
```

如你所见，与 store/3 函数的先前版本相比，唯一的变化是在将 changeset 插入数据库之前使用 Changeset.put_assoc/3。通过它，我们在 changeset 中添加了新 Media 条目与在第一个 with 步骤中从数据库获取的 Owner 之间的关联：

iex> changeset = Media.create_initial_changeset(owner.id, file_name, full_path, file_size)
#Ecto.Changeset<
action: nil,
changes: %{
  # ...
},
errors: [],
data: #ElixirDrip.Storage.Media<>, 
valid?: true
>

iex> changeset = Changeset.put_assoc(changeset, :owners, [owner])
#Ecto.Changeset<
    action: nil,
    changes: %{
        # ...
        owners: [
            #Ecto.Changeset<action: :update, changes: %{}, errors: [],
            data: #ElixirDrip.Storage.Owner<>, valid?: true>
        ]
    },
    errors: [],
    data: #ElixirDrip.Storage.Media<>, 
    valid?: true
>

通过使用 Changeset.put_assoc/3 函数，Ecto 将为我们更新 Media 模式的 owners 字段。此函数期望我们传递整个关联（在我们的情况下，是给定媒体的所有所有者），然后它将相应地更新它。在这里我们正在创建新的媒体，因此我们确定它只有一个所有者（因此列表中只有一个所有者元素）。

现在考虑一个之前与 prev_owner 用户关联的媒体。如果您为其创建了一个 changeset 并尝试执行 put_assoc(changeset, :owners, [prev_owner, new_owner])，您将得到一个有效的 changeset，因为我们只是添加了一个新的所有者。然而，如果我们尝试通过传递 [new_owner] 作为新的所有者列表来更改现有所有者（我们试图删除与 prev_owner 的现有关联），默认情况下 Ecto 将引发错误。Ecto 在此场景中的反应方式由在定义 many_to_many 字段时使用的 :on_replace 选项控制。如果我们希望 Ecto 为我们删除陈旧的关联，我们应该将此选项设置为 :delete。

现在让我们尝试完整的 Storage.store/3 函数。如你所见，在获取我们传递的 ID 的所有者后，Ecto 按预期在 storage_media 和 media_owners 表上插入新行。请注意 Ecto 甚至将这两个插入包装在单个事务中，确保无论发生什么情况，我们数据库的一致性：

iex> xpto_user = Accounts.get_user_by_username("xpto")
%ElixirDrip.Accounts.User{
  __meta__: #Ecto.Schema.Metadata<:loaded, "users">,
  id: "13ix90kpmblgJF4Ful762gqb6m2",
  # ...
}

iex> Storage.store(xpto_user.id, "hello_world.txt", "$/hello/world", "Hi there, this is a test.")

19:39:17.617 [debug] QUERY OK source="users" db=0.6ms
SELECT u0."id", u0."email" FROM "users" AS u0 WHERE (u0."id" = $1)
["13ix90kpmblgJF4Ful762gqb6m2"]

19:39:17.618 [debug] QUERY OK db=0.1ms queue=0.1ms
begin []

19:39:17.634 [debug] QUERY OK db=15.9ms
INSERT INTO "storage_media"
("encryption_key","file_name","file_size","full_path","id","metadata","storage_key","user_id","inserted_at","updated_at") VALUES ($1,$2,$3,$4,$5,$6,$7,$8,$9,$10) [...] 

19:39:17.636 [debug] QUERY OK db=1.9ms
INSERT INTO "media_owners"
("media_id","user_id","inserted_at","updated_at") VALUES ($1,$2,$3,$4)
RETURNING "id" ["13j6fLDkPbkbq6Fo2Cuyb2MF4s", "13ix90kpmblgJF4Ful762gqb6m2", ...]

19:39:17.640 [debug] QUERY OK db=3.2ms
commit []
# ...

我们的 ElixirDrip 用户也将能够与其他用户共享他们的媒体。每次共享文件时，我们都需要更新媒体-用户关联。然而这次，我们将直接操作 MediaOwners 连接模式：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/storage.ex
defmodule ElixirDrip.Storage do
  # ...
  def share(user_id, media_id, allowed_user_id) do
    with {:ok, :creator} <- is_creator?(user_id, media_id) do
      %MediaOwners{}
      |> Changeset.cast(%{user_id: allowed_user_id, media_id: media_id}, [:user_id, :media_id])
      |> Repo.insert()
    else
      error -> error
    end
  end
end
```

在 Storage.share/3 函数中，我们首先确保共享媒体的用户是最初上传该媒体的用户（is_creator?/2 函数将在本章后面分析）。如果用户是媒体创建者，我们将继续创建并插入一个将 media_id 与 allowed_user_id 关联的 changeset，允许新用户查看和下载相关媒体。通过在 MediaOwners 连接模式中创建新条目，我们允许更多用户访问给定的媒体文件。

如果我们也想在这里使用 Changeset.put_assoc/3 方法，我们必须首先获取所有现有媒体所有者的列表，然后将新的允许用户添加到该列表中。尽管这是一个有效的替代方案，但这种方法意味着额外的数据库查询来获取现有所有者。

本节分析的一些操作使用底层数据库约束来确保我们数据的一致性。在下一节中，我们将了解 Ecto 如何利用数据库约束并允许我们轻松地使用它们。

## 约束

当我们分析 User.create_changeset/2 函数时，我们没有设置任何验证来检查我们存储在数据库中的用户名是否已存在。对于我们这样的系统，这可能意味着麻烦。因此，让我们为 users 表的 username 列创建一个唯一索引：

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

在我们使用 mix ecto.migrate 应用此迁移后，数据库将开始在 users 表的任何操作上强制执行用户名唯一性。让我们看看如果我们尝试将 xpto_user 用户名更新为已存在的用户名会发生什么：

iex> xpto_user
%ElixirDrip.Accounts.User{ ... }

iex> update_op = Changeset.cast(xpto_user, %{username: "jose"},
  [:username])
#Ecto.Changeset<
action: nil,
changes: %{username: "jose"},
errors: [],
data: #ElixirDrip.Accounts.User<>, 
valid?: true
>

iex> Repo.update(update_op)
07:43:57.503 [debug] QUERY ERROR db=29.6ms queue=0.1ms
UPDATE "users" SET "username" = $1, "updated_at" = $2 WHERE "id" = $3
["jose", {{2018, 4, 26}, {7, 43, 57, 470077}},
"13ix90kpmblgJF4Ful762gqb6m2"]
** (Ecto.ConstraintError) constraint error when attempting to update struct:
  * unique: users_username_index
  # ...

正如预期的那样，数据库不允许更新并且 Ecto 引发了约束错误。尽管正确，但这种行为对我们帮助不大，因为我们通常使用 valid? 和 errors changeset 字段来检查是否发生错误。为了解决这种情况，Ecto 提供了 Changeset.unique_constraint/3 函数，该函数在执行数据库操作时检查唯一性约束，如果操作违反了现有的约束，则返回一个 {:error, changeset} 元组。

让我们将此 Changeset.unique_constraint/3 应用于 User.create_changeset/2 函数，我们在创建新的 ElixirDrip 用户时使用它：

```
$ cat apps/elixir_drip/lib/elixir_drip/accounts/user.ex
defmodule ElixirDrip.Accounts.User do
  use Ecto.Schema
  import Ecto.Changeset
  # ...
  def create_changeset(%User{} = user, attrs) do
    user
    |> cast(attrs, [:username, :password, :email])
    |> validate_required([:username, :password, :email])
    |> validate_length(:username, min: 1, max: 30)
    |> validate_length(:password, min: 8, max: 100)
    |> validate_format(:email, @email_regex)
    |> unique_constraint(:username)
    |> put_hashed_password()
  end
end

