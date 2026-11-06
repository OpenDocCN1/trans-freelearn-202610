![](bbox=[0.12, 0.11, 0.21, 0.2])

To simplify the nomenclature, throughout this book we will explicitly use the terms *umbrella project* and *umbrella application* to refer to the root project and each of its contained applications, respectively.

To illustrate how umbrella projects make our life easier, let's start by laying the foundation of the project we will build throughout this book. In the following snippet, we are creating a new `elixir_drip` umbrella project with an `ElixirDrip.Umbrella` top module. We are also naming the project OTP application as `elixir_drip_umbrella`:

```
$ mix new elixir_drip --module ElixirDrip.Umbrella --app elixir_drip_umbrella --umbrella

* creating .gitignore
* creating README.md
* creating mix.exs
* creating apps
* creating config
* creating config/config.exs
```

Your umbrella project was created successfully.
Inside your project, you will find an apps/ directory where you can create and host many apps:

```
cd elixir_drip
cd apps
mix new my_app
```

```
Commands like "mix compile" and "mix test" when executed in the umbrella project root will automatically run for each application in the apps/ directory.
```

![](bbox=[0.12, 0.65, 0.21, 0.74])

Adding the *umbrella* reference to both module and application names makes it clearer this is in fact an umbrella project and will let us use, further ahead, the name `elixir_drip` for the umbrella application that will be responsible for the business logic.

We can now analyze the generated `mix.exs` file. As you can see, we have a new `apps_path` entry returned by the `project/0` function, and we have no dependencies at all. Mix uses this to know where the umbrella applications live. Also notice the module's name: the `--module ElixirDrip.Umbrella` option we passed with the `mix new` command set its prefix.