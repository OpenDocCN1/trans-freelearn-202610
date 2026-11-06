```
def get_or_default([], _key, default), do: default
def get_or_default(options, key, default) do
  case options[key] do
    nil -> default
    value -> value
  end
end

# to generate random names for each process and pipeline
defp random_name(name), do: Module.concat(name, random_suffix())
defp random_suffix, do: "p" <> (:crypto.strong_rand_bytes(4) |>
Base.encode16())
end
```

As you can see, we are employing the use/2 and __using__/1 macros to inject into the caller module the generated module attributes with the default GenStage parameters. We are temporarily injecting the show_default_values/0 function only to assert that we are setting the expected values. In the following code snippet, we define two candidate modules, using the current version of the ElixirDrip.Pipelin er; the former is not defining any parameter and the latter is defining all the available parameters:

```
iex> defmodule Foo do
...> use ElixirDrip.Pipelin er
...> end
Building Pipeliner: Elixir.pipelin er.P95BE84C3

{:module, Foo,
<<70, 79, 82, ...>>, {:show_default_values, 0}}

iex> Foo.show_default_values
Defaults: count=1, min_demand=1, max_demand=10
:ok
```

```
iex> defmodule Bar do
...> use ElixirDrip.Pipelin er, name: :bar, min_demand: 3, max_demand: 5,
count: 4
...> end
Building Pipeliner: bar

{:module, Bar,
<<70, 79, 82, ...>>, {:show_default_values, 0}}

iex> Bar.show_default_values
Defaults: count=4, min_demand=3, max_demand=5
```