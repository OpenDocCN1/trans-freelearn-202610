Otherwise, the module name prefix would be the camel-case version of the project name passed to the mix new <project_name> task (ProjectName, that is). Take a look at this code:

```
$ cat mix.exs
defmodule ElixirDrip.Umbrella.Mixfile do
  use Mix.Project

  def project do
    [
      apps_path: "apps",
      start_permanent: Mix.env == :prod,
      deps: deps()
    ]
  end

  # Dependencies listed here are available only for this
  # project and cannot be accessed from applications inside
  # the apps folder.
  #
  # Run "mix help deps" for examples and options.
  defp deps do
    []
  end
end
```

We now have our first umbrella project ready for action. Sadly, it does not have any umbrella applications yet:

```
$ tree
.
├── apps
├── config
│   └── config.exs
├── deps
├── mix.exs
└── README.md

3 directories, 3 files
```

If you try to run it using the iex -S mix command, nothing will happen; you will only get the idleness of the Elixir interactive shell waiting for your input. Let's fix this by creating the first umbrella application of our project.