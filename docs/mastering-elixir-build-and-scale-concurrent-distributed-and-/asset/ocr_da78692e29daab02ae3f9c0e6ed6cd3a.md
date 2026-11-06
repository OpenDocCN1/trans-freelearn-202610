In our case we'll use the `postgrex` library, which provides the PostgreSQL Elixir driver. But don't think Ecto is tied to PostgreSQL in any way, because Ecto supports other databases like MySQL, DynamoDB, and SQLite, among others.

Let's start by adding the `ecto` and `postgrex` libraries to our `elixir_drip` umbrella app dependencies:

```
$ cat apps/elixir_drip/mix.exs
defmodule ElixirDrip.Mixfile do
  use Mix.Project

  defp deps do
    [
      {:postgrex, "~> 0.13"},
      {:ecto, "~> 2.1"},
      # ...
    ]
  end
end
```

These two dependencies will be fetched by `mix` as soon as we run `mix deps.get`. The next step consists of defining and configuring the Ecto repository. Every database operation passes through it, and it's the responsibility of the repository to ask the underlying database driver to interact with the database on its behalf.

![](bbox=[0.12, 0.58, 0.2, 0.68])

The repository will use an Ecto adapter, in our case `Ecto.Adapters.Postgres`, and it's this adapter that will interact with the `postgrex` driver.

The following `ElixirDrip.Repo` module materializes our Ecto repository:

```
$ cat apps/elixir_drip/lib/elixir_drip/repo.ex
defmodule ElixirDrip.Repo do
  use Ecto.Repo, otp_app: :elixir_drip

  def init(_, opts) do
    {:ok, Keyword.put(opts, :url, System.get_env("DATABASE_URL"))}
  end
end
```

By using `Ecto.Repo`, we will be able to perform all the database operations through the `ElixirDrip.Repo` module. The `:otp_app` option is mandatory and indicates which application has the repository configuration; in our case, the repository configuration exists under the `elixir_drip` application configuration.