
```
def start_link(name, type) do
  GenStage.start_link(__MODULE__, type, name: name)
end

def init(type) do
  Logger.debug("#{inspect(self())}: #{type} Pipeline Starter started.")

  {
    :producer,
    %{queue: QueueWorker.queue_name(type), type: type, pending: 0}
  }
end
# ...
end
```

We can see a pattern here: the name argument is only used by the start_link/2 function to set the producer process name when we start it, whereas the type argument is simply passed to the init/1 function via the GenStage.start_link/3 call, and it's used to initialize the process state. In a sense, we can consider the name argument as required because the process start depends on it and the type argument as optional, since it only contributes for the initial state of the process. In the end, the init/1 function returns a two element tuple with :producer and the initial process state.

Let's create a Pipeliner.Producer macro module that helps in the definition of the two previous functions. We will apply again the use/__using__/1 construct to indicate what the optional arguments are. The preceding code snippet will be simplified into the following:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/starter.ex
defmodule ElixirDrip.Storage.Pipeline.Starter do
  require Logger
  alias ElixirDrip.Storage.Workers.QueueWorker

  use ElixirDrip.Pipeliner.Producer, args: [:type]

  @impl ElixirDrip.Pipeliner.Producer
  def prepare_state([type]) do
    Logger.debug("#{inspect(self())}: #{type} Pipeline Starter started.")

    %{queue: QueueWorker.queue_name(type), type: type, pending: 0}
  end
  # ...
end
```

Looking at the end result, we can understand this module is a producer with one optional argument that needs to execute some additional steps to prepare the initial process state. Behind the scenes, a `start_link/2` function that sets a random name for the process and an `init/1` function that calls the `prepare_state/1` function are being set for us by the `Pipeliner.Producer` module.

We rely on the `Pipeliner.get_or_default/3` function to fetch the `args` passed via the `__using__/1` options, and we then create the optional and required arguments, using the `create_args/2` function. This function relies on the `Macro.var/2` function to generate the quoted representation of each variable.

![](img/c636f4cfb07f25f1db535995e50706ed_282_0.png)

Remember that every quoted representation is a, possibly nested, three-element tuple. Therefore, function variables are also represented in the same way. Doing `Macro.var(:name, __MODULE__)` will yield `{:name, [], __MODULE__}`, the variable quoted representation, ready to be injected into another AST.

Given that the generated `start_link` function will have to receive both optional and required arguments, we merge them into a new `function_args` variable. The number of arguments this function receives changes proportionally to the number of optional arguments we have. In this kind of situation, the `unquote_splicing/1` is really useful because it unquotes in place each element of the list it receives, making it the perfect aid when defining a function with variable arguments.

In this example, the `function_args` variable will contain `[{:type, [], __MODULE__}, {:name, [], __MODULE__}]`; that is to say, the quoted representation of the `type` and `name` variables.

When we define the `start_link` function with the `unquote_splicing(function_args)` arguments, the function arguments will effectively become `unquote(type)`, `unquote(name)`. On the `start_link` function body, we use the `name` argument as the new process name and pass the `optional_args` list to the `init/1` function, via the second argument of the `GenStage.start_link/3` function:

```elixir
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliners_producer.ex
defmodule ElixirDrip.Pipeliner.Producer do
  import ElixirDrip.Pipeliner
  # ...
  defmacro __using__(opts) do
    args = get_or_default(opts, :args, [])

    optional_args = create_args(__MODULE__, args)
    required_args = create_args(__MODULE__, [:name])
```

```
function_args = optional_args ++ required_args

quote do
  use GenStage
  # ...
  def start_link(unquote_splicing(function_args)) do
    GenStage.start_link(__MODULE__, unquote(optional_args), name: name)
  end
  # ...
end
end
```

```
defp create_args(_, []), do: []
defp create_args(module, arg_names),
  do: Enum.map(arg_names, &Macro.var(&1, module))
end
```

Let's now analyze how the init/1 function is generated. As we previously saw, the optional_args list is passed to the init/1 function (notice the enclosing [...] used to pattern-match on the single argument of the init/1 function). As such, this function receives the list and then calls the default implementation of the prepare_state/1 function, which is also injected into the caller module by the Pipeliner.Producer module. In the end, it returns the expected {:producer, state} tuple that the GenStage behaviour requires:

```
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliner_producer.ex
defmodule ElixirDrip.Pipeliner.Producer do
  import ElixirDrip.Pipeliner

  @callback prepare_state(list(any)) :: any

  defmacro __using__(opts) do
    # ... optional_args, required_args and function_args definition

    quote do
      use GenStage
      @behaviour unquote(__MODULE__)
      @behaviour GenStage
      # ...

      @impl GenStage
      def init([unquote_splicing(optional_args)]) do
        args = prepare_state(unquote(optional_args))

        {:producer, args}
      end
```

```
```elixir
@impl unquote(__MODULE__)
def prepare_state(args), do: args

deoverridable unquote(__MODULE__)
end
end
# ...
end
```
```

The default implementation of the prepare_state/1 function consists of an identity function because it returns exactly the arguments it receives. To allow the caller module to implement its own version of the prepare_state/1 function and override the default version, we define the prepare_state/1 function as a @callback, and we then say that it is possible to override the functions of the module being injected with deoverridable Pipeliner.Producer. By calling the deoverridable/1 with a module, we are saying that every implemented callback function of the indicated module is overridable. In our case, given that the only callback is the prepare_state/1 function, we could have designated the specific callback by using deoverridable [prepare_state: 2] instead.

We are using the @impl Foo annotations to indicate to the compiler that those functions are implementing a callback from the Foo module. One of the benefits of doing this is that, once the compiler sees one @impl annotation, it will check whether the function name and arity of all the overriding functions match the signature of the respective behaviour callbacks. Using the @impl mechanism once is enough for the compiler to check every behaviour the module implements. So if we omit the @impl GenStage from the init/1 function we are injecting on the caller module, the following warning would be raised:

```
```
iex> recompile
==> elixir_drip
Compiling 12 files (.ex)
```
```

```
```
warning: module attribute @impl was not set for function init/1 callback
(specified in GenStage). This either means you forgot to add the "@impl
true" annotation before the definition or that you are accidentally
overriding this callback
lib/elixir_drip/storage/pipeline/starter.ex:24
```
```

Let's now apply the same approach to simplify our pipeline consumers.

## Simpler pipeline (producer) consumers

The approach we'll now adopt is really similar to the previous one for producer stages.
Currently, this is what the RemoteStorage stage looks like:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/
remote_storage.ex
defmodule ElixirDrip.Storage.Pipeline.RemoteStorage do
  use GenStage
  require Logger
  alias ElixirDrip.Storage
  alias Storage.Provider
  alias Storage.Supervisors.CacheSupervisor, as: Cache

  @dummy_state []

  def start_link(name, subscription_options),
    do: GenStage.start_link(__MODULE__, subscription_options, name: name)

  def init(subscription_options) do
    Logger.debug("#{inspect(self())}: Pipeline RemoteStorage started. Options: #{inspect(subscription_options)}")

    {:producer_consumer, @dummy_state, subscription_options}
  end
  # ...
  end
```

Let's see the end result applied to the same RemoteStorage stage, which is a producer consumer. The main difference here lies in the additional type option passed via the use options (the module we'll create will be used by both producer consumers and consumers) and the new subscription options, both needed by the init/1 callback:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/
remote_storage.ex
defmodule ElixirDrip.Storage.Pipeline.RemoteStorage do
  require Logger
  alias ElixirDrip.Storage
  alias Storage.Provider
  alias Storage.Supervisors.CacheSupervisor, as: Cache

  use ElixirDrip.Pipeliners.Consumer, type: :producer_consumer

  @impl ElixirDrip.Pipeliners.Consumer
  def prepare_state(_dummy_state) do
    Logger.debug("#{inspect(self())}: Streamlined Pipeline RemoteStorage started.")
```
