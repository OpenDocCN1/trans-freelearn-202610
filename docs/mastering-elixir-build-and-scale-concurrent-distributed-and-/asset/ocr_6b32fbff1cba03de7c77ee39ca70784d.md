We're running a PostgreSQL database on port 5432 in our current development environment, so we've set the values of the DB_* environment variables as follows:

```
$ env | grep DB_
DB_PORT=5432
DB_NAME=elixir_drip_dev
DB_PASS=123456
DB_HOST=localhost
DB_USER=postgres
```

As we saw in the development configuration file, these environment variables are being used to set all the required configurations to start ElixirDrip.Repo.

With the previous configuration in place, we're just missing one last piece of the puzzle to get the repository up and running: we need to actually start the repository as a supervisor. To do this, we'll edit the ElixirDrip.Application module and add the ElixirDrip.Repo supervisor as one more child that will be started by the elixir_drip root supervisor:

```
$ cat apps/elixir_drip/lib/elixir_drip/application.ex
defmodule ElixirDrip.Application do
  use Application

  def start(_type, _args) do
    import Supervisor.Spec, warn: false

    Supervisor.start_link(
      [
        supervisor(ElixirDrip.Repo, []),
        # ...
      ],
      strategy: :one_for_one,
      name: ElixirDrip.Supervisor
    )
  end
end
```

We will now finally interact with the database! Let's start by creating the database with the ecto.create Mix task. This task will create the database given by the :database entry of the ElixirDrip.Repo configuration on the dev Mix environment:

```
$ echo $MIX_ENV
dev

$ mix ecto.create
The database for ElixirDrip.Repo has been created
```