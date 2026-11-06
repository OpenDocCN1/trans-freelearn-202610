
```
end
end

defp verify_user_password(%User{} = user, password) do
  Comeonin.Bcrypt.checkpw(password, user.hashed_password)
end
end
```

在找到具有给定用户名的用户后，我们调用 verify_user_password/2 函数来检查密码是否有效。如果密码不匹配，我们调用 Comeonin.Bcrypt.dummy_checkpw/0 函数。此函数返回所需的时间不是恒定的，因此攻击者无法对我们的登录系统执行时序攻击。

在 Accounts 上下文中保留之前的 User schema 完全合理，但也产生了一个需要解决的问题。因为该 schema 位于 Accounts 上下文中，Storage 上下文不允许与之交互。但我们仍然需要在 Storage 端拥有用户的概念，因为所有媒体文件都由一个或多个用户拥有。

为了解决这个问题，让我们定义一个新的 Owner schema，位于 Storage 上下文中，它也由同一个 users 表支持：

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/owner.ex
defmodule ElixirDrip.Storage.Owner do
  use Ecto.Schema

  @primary_key {:id, ElixirDrip.Ecto.Ksuid, autogenerate: true}
  schema "users" do
    field :email, :string
  end
end
```

如果你有 ORM 工具的使用经验，你可能会觉得为同一个数据库表设置多个 schema 很奇怪。然而，采用分离的有界上下文并使用最终反映同一数据库表的独立 schema 的方法，有助于我们避免拥有一个需要服务于所有目的的庞大 schema。这样，我们可以拥有用于用户管理的 schema、用于媒体拥有的 schema，以及用于审计目的的 schema 等，每个 schema 对用户都有自己的视角。

Schema 非常有用，但它们不是强制性的。因此，你如何决定是否需要它们呢？

## Schema 与否——这就是问题

到目前为止，我们谈论 schema 时似乎它们在使用 Ecto 实现数据库访问时是强制性的。在 Ecto 2.0 之前，这在某种程度上是正确的，因为没有 schema 的许多数据库查询比使用 schema 的对应查询更冗长，而且某些数据库操作只有在选择 schema 方式时才可能。

幸运的是，那些日子已经过去了，现在你可以在没有 schema 的情况下执行所有操作和查询。例如，假设我们有一个 `users` 数据库表，其中包含 `:username` 和 `:email` 字段。你可以通过调用 `Repo.insert_all/2` 函数并传递一个列表的列表，例如 `[[username: "andre", email: "aa@elixir.pt"], [username: "daniel", email: "dc@elixir.pt"]]`, 来在此表上插入两个新条目，表示要插入的两行。

当你想从数据库检索数据并且查询涉及大量表连接时也是如此。是否值得尝试将所有结果适配到查询中涉及的不同 schema 中？可能不是。最好只是将结果映射到 Elixir 标准数据结构（比如一个 map），然后结束。

这里的关键要点是，你可以为同一个数据库表拥有多个 schema，也可以没有。这取决于你的需求，而且幸运的是 Elixir 使你在之后更改它变得很容易。想象一下，你对某个表没有 schema，你的查询返回的是一个 map 列表。如果你决定添加一个 `Foo` schema，只需做最小的更改，你的查询现在就可以返回 `Foo` 结构列表。之前 map 列表的现有模式匹配可能仍然有效，因为 Elixir struct 实际上只是一个特殊的 map。

我们定义的 `Storage.Media` schema 将由数据库表支持。让我们看看如何使用 Ecto `migrations` 创建它。

## 迁移

正如我们已经看到的，我们的 `Media` schema 的目的是在 Elixir 世界中表示从数据库检索的数据。由于我们使用的是关系数据库，数据会持久化在数据库表中。

为了创建表，而不是直接在数据库上执行 SQL 脚本，我们将使用 Ecto migrations 来为我们应用每个数据库 schema 更改。

Migrations 以 Elixir 脚本的形式存放在 priv/repo/migrations 文件夹中，其名称应遵循 <timestamp>_<snake_case_name>.exs 格式，例如 20180112095312_create_media.exs。要执行迁移或回滚现有迁移，你应分别使用 ecto.migrate 或 ecto.rollback。

以下是负责创建 storage_media 数据库表的 CreateMedia 迁移：

```
$ cat apps/elixir_drip/priv/repo/migrations/
20180112095312_create_media.exs
defmodule ElixirDrip.Repo.Migrations.Storage.Media do
  use Ecto.Migration

  def change do
    create table(:storage_media, primary_key: false) do
      add :id, :string, primary_key: true, size: 27
      add :file_name, :string
      add :full_path, :string
      add :metadata, :map
      add :encryption_key, :string
      add :storage_key, :string
      add :uploaded_at, :utc_datetime

      timestamps(type: :utc_datetime)
    end
  end
end
```

如你所见，迁移由一个使用 Ecto.Migration 模块的普通模块组成。在这里你可以看到我们使用 create/2 宏和 table/2 函数来创建 storage_media 表。

因为我们在迁移模块上实现了 change/0 函数，Ecto 预计在我们运行 ecto.rollback Mix 任务时能够自动撤销操作，而无需我们明确指示撤销操作。

如果你愿意，或者如果 Ecto 无法自动判断如何回滚迁移，你可以改为实现 up/0 和 down/0 函数，它们将在分别执行 ecto.migrate 和 ecto.rollback Mix 任务时运行。

我们在 table/2 调用中将 :primary_key 选项设为 false，因为我们希望之后显式地将其定义为 27 个字符的字符串字段。table/2 函数将返回一个表示将要创建的表的 Table struct，而 create/2 宏将接收 Table struct 和字段列表，并创建相应的表。

让我们运行迁移，然后使用 ecto.migrations Mix 任务检查其状态：

```
$ mix ecto.migrate
18:16:18.548 [info] == Running
ElixirDrip.Repo.Migrations.Storage.Media.change/0 forward
18:16:18.548 [info] create table storage_media
18:16:18.572 [info] == Migrated in 0.0s

$ mix ecto.migrations

Repo: ElixirDrip.Repo

| Status | Migration ID | Migration Name |
|---|---|---|
| up | 20180112095312 | create_media |
```

如你所见，我们的 create_media 迁移已正确应用。我们现在将定义 schema 和迁移，以实现我们用户与其媒体之间的关系。

## 关系

我们此处的目标是实现 Media 和 Owner schema 之间的关系。一开始，媒体文件只有一个所有者：上传媒体到系统中的用户。该用户随后可以与其它用户共享。因此，媒体文件可能由一个以上用户拥有，并且肯定至少由一个用户拥有。

在此关系的另一侧，我们有媒体所有者。最近创建的用户可能没有任何媒体文件，但最终它将拥有数十个文件：

![](img/c636f4cfb07f25f1db535995e50706ed_305_0.png)

用数据库术语来说，我们有一个经典的多对多关系。因此，我们需要一个额外的关联表，因为我们已经有 media 和 users 数据库表。让我们使用迁移创建 media_owners 表：

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

此迁移创建 media_owners 表，其中包含两个字符串列 media_id 和 user_id。通过对两个字段使用 references/2 宏，它们将被定义为外键。我们放置在 media_id 和 user_id 列上的每个值都需要在 storage_media 和 users 表的 id 列上存在，分别对应。我们不需要使用 name 选项来指示被引用表的 primary key，因为它默认为 :id，这也是我们两个表上 primary key 字段的名称。
