```
# returning the quoted block macro result
end
end
# ...
end
```

In the previous code snippet, we started by validating the `type` option passed through the `use/2` options. Afterward, we will `unquote` the `type` option as the first element of the three-element tuple returned by the `init/1` callback, to indicate whether this stage is a `:producer_consumer` or a `:consumer` stage:

```
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliner_consumer.ex
defmodule ElixirDrip.Pipeliners.Consumer do
  import ElixirDrip.Pipeliners
  # ...
  defmacro __using__(opts) do
    # ...
    quote do
      use GenStage
      @behaviour unquote(__MODULE__)
      @behaviour GenStage

      # we are passing the optional_and_subscription_args
      # to the init/1 function via the GenStage.start_link/3 function
      def start_link(unquote_splicing(function_args)) do
        GenStage.start_link(
          __MODULE__, unquote(optional_and_subscription_args), name: name)
      end

      @impl GenStage
      def init([unquote_splicing(optional_and_subscription_args)]) do
        state = prepare_state(unquote(optional_args))

        # injecting the stage type
        {unquote(type), state, subscribe_to: sub_options}
      end

      @impl unquote(__MODULE__)
      def prepare_state(args), do: args

        defoverridable unquote(__MODULE__)
      end
    end
    # ...
  end
end
```