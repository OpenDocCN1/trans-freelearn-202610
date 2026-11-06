Let's focus ourselves on the start/2 macro, given that all the mentioned macros are equivalent. The only difference lies in the first element of the returned keyword list: here, it has a :producer key, whereas on the step/2 and finish/2 macros, the key is a :producer_consumer and :consumer atom, respectively:

```
```elixir
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliner.ex
defmodule ElixirDrip.Pipeliners do
  # ...
  defmacro start(producer, opts \\ []) do
    quote bind_quoted: [producer: producer, opts: opts] do
      options_and_args = get_options_and_args(opts, @default_count,
      @default_min_demand, @default_max_demand)

      @pipeline_steps [producer: producer] ++ options_and_args
      IO.puts "START: #{producer}, #{inspect(options_and_args)}"
    end
  end

  def get_options_and_args(opts, default_count, default_min_demand,
  default_max_demand) do
    options_and_args = fill_options_and_args(opts, default_count,
    default_min_demand, default_max_demand)

    options = Keyword.drop(options_and_args, [:args])
    [
      args: options_and_args[:args],
      options: options
    ]
  end

  def fill_options_and_args(options, default_count, default_min_demand,
  default_max_demand) do
    [{:args, []},
    {:count, default_count},
    {:min_demand, default_min_demand},
    {:max_demand, default_max_demand}]
    |> Enum.reduce([], fn {key, default}, result ->
      Keyword.put(result, key, get_or_default(options, key, default))
    end)
  end
end
```
```