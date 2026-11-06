
```
[ Step1Module.P44712585, # name [ # subscriptions {StartModule.P5EDD23C9, [max_demand: 6, min_demand: 4]}, {StartModule.P12858ABF, [max_demand: 6, min_demand: 4]} ], ]}, :permanent, 5000, :worker, [Step1Module]}, {"Elixir.Step1Module.P68393018", ...}, {"Elixir.Step1Module.P0F918D60", ...} ] }
```

As you can see, the function returned three Step1Module worker specs named Step1Module.P44712585, Step1Module.P68393018 and Step1Module.P0F918D60, each subscribing to the two StartModule processes passed via the :names_to_subscribe keyword. Given the Step1Module stage didn't define any arguments, only the name and subscriptions will be passed to the Step1Module.start_link/2 function.

Before we move on, we need to create a similar get_worker_specs/1 function that pattern-matches on the :consumer keyword entry instead:

```
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliner.ex
defmodule ElixirDrip.Pipeliner do
  import Supervisor.Spec
  # ...
  def get_worker_specs(producer_consumer: producer_consumer,
    args: args, options: options, names_to_subscribe:
    names_to_subscribe) do
    get_worker_specs_with_subscriptions(producer_consumer,
      args: args, options: options, names_to_subscribe:
      names_to_subscribe)
  # ...
  end
```

We are now able to tackle the following phase: how to collect the worker specs of each stage.

## Collecting the pipeline worker specs

The goal is now to generate and aggregate the worker specs of each stage, while also passing them the process names of the previous stage (except on the first producer stage, which doesn't subscribe to any stage).

This is exactly what the following pipeline_specs/1 function does: it starts by reducing all the steps it receives. When the resulting worker_specs is empty, it means we are looking at the first producer step, so we simply add the outcome of get_worker_specs(step) to the end result. If it isn't empty, it means we are processing a ProducerConsumer or a Consumer stage, so we need to add the names_to_subscribe keyword to the step before getting its worker specs:

```
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliner.ex
defmodule ElixirDrip.Pipeliner do
  import Supervisor.Spec
  # ...
  def pipeline_specs(steps) do
    steps
    |> Enum.reduce([], fn step, worker_specs ->
      step = case Enum.empty?(worker_specs) do
        true -> step
        _ ->
          {names_to_subscribe, _} = Enum.at(worker_specs, -1)
          step ++ [names_to_subscribe: names_to_subscribe]
      end
      worker_specs ++ [get_worker_specs(step)]
    end)
    |> Enum.unzip()
    |> elem(1)
    |> List.flatten()
  end
end
```

The Enum.reduce/2 result is a list where each element is a tuple with the worker name and its spec, so we unzip it to get a tuple with a list of all the worker names in one element and a list of all the worker specs in the other, grab the list with the worker specs, and flatten it. We finally have the worker specs ready to be parsed by the pipeline supervisor! Take a look at the following:

```
iex> Qaz.show_pipeline_steps |> ElixirDrip.Pipeliner.pipeline_specs
[
  # two StartModule processes
  {"Elixir.StartModule.PB6AD0F42",
    {StartModule, :start_link, [:start_arg1, :start_arg2,
    StartModule.PB6AD0F42]},
    :permanent, 5000, :worker, [StartModule]},
  {"Elixir.StartModule.PDD3D519D", ...},

  # three Step1Module processes subscribing
  # to the two previous stage processes
  {"Elixir.Step1Module.PAEC81181",
{Step1Module, :start_link,
  [
    Step1Module.PAEC81181,
      [ # two subscriptions for each process
        {StartModule.PB6AD0F42, [max_demand: 6, min_demand: 4]},
        {StartModule.PDD3D519D, [max_demand: 6, min_demand: 4]}
      ]
  ], :permanent, 5000, :worker, [Step1Module]},
{"Elixir.Step1Module.PAEC1AE45", ...},
{"Elixir.Step1Module.PAEC5EBB3", ...},

# five Step2Module processes subscribing
# to the previous three stage processes
{"Elixir.Step2Module.P08E72352",
{Step2Module, :start_link,
  [
    Step2Module.P08E72352,
      [ # three subscriptions for each process
        {Step1Module.PAEC81181, [max_demand: 10, min_demand: 1]},
        {Step1Module.PAEC1AE45, [max_demand: 10, min_demand: 1]},
        {Step1Module.PAEC5EBB3, [max_demand: 10, min_demand: 1]}
      ]
  ], :permanent, 5000, :worker, [Step2Module]},
{"Elixir.Step2Module.P1470075A", ...},
{"Elixir.Step2Module.P9200FBE7", ...},
{"Elixir.Step2Module.PD4EB8A27", ...},
{"Elixir.Step2Module.P7A005C99", ...},

# one FinishModule process subscribing
# to the previous five stage processes
{"Elixir.FinishModule.PA2194E2F",
{FinishModule, :start_link,
  [
    FinishModule.PA2194E2F,
      [
        {Step2Module.P08E72352, [max_demand: 10, min_demand: 1]},
        {Step2Module.P1470075A, [max_demand: 10, min_demand: 1]},
        {Step2Module.P9200FBE7, [max_demand: 10, min_demand: 1]},
        {Step2Module.PD4EB8A27, [max_demand: 10, min_demand: 1]},
        {Step2Module.P7A005C99, [max_demand: 10, min_demand: 1]}
      ]
  ], :permanent, 5000, :worker, [FinishModule]}
]
```

If we pass all the worker specs mentioned previously to a Supervisor.init/2 function, it will happily start all the 11 pipeline processes, assuming the StartModule, Step1Module, Step2Module, and FinishModule modules exist and implement the needed GenStage callback functions; that is to say, handle_demand/2 for a Producer stage and handle_events/3 for a ProducerConsumer or Consumer stage.

Let's now define the pipeline supervisor functions, so we can have a fully functional DSL!

## Defining the supervisor functions

Given that all the hard work has already been done in the previous sections, we just need to inject in the caller module the start_link/0 and init/1 functions, so it can be started as a normal supervisor:

```
$ cat apps/elixir_drip/lib/elixir_drip/dsl/pipeliner.ex
defmodule ElixirDrip.Pipeliner do
  import Supervisor.Spec
  # ...
  defmacro __using__(opts) do
    # ...
    IO.puts "Building Pipeliner: #{name}"
    quote do
      # ...
      Module.register_attribute(__MODULE__, :pipeline_steps,
        accumulate: true)

      @before_compile unquote(__MODULE__)

      def start_link(),
        do: Supervisor.start_link(__MODULE__, [], name: unquote(name))

      def init(_) do
        worker_specs_to_start = get_pipeline_steps()
          |> pipeline_specs()

        Supervisor.init(
          worker_specs_to_start,
          strategy: :rest_for_one,
          name: __MODULE__
        )
      end

      def show_pipeline_steps do
        # ...
      end
    end
  end
end
```

```
end
end
end
```

On the start_link/0 function, we simply start the supervisor with the name we previously obtained. On the init/1 function, we fetch the pipeline steps, convert them to worker specs, and then initialize the supervisor with the mentioned worker specs.

Notice that most of the functions used for the DSL aren't injected on the caller module. We opt instead to call the functions that live on the ElixirDrip.Pipeliner module (the caller module can access those because it imports the Pipeliner module). This is a good practice that aims to avoid cluttering the caller module with functions that are only needed in compile time for macro expansion.

![](img/c636f4cfb07f25f1db535995e50706ed_280_0.png)

To see the full code of the ElixirDrip.Pipeliner module, check the apps/elixir_drip/lib/elixir_drip/dsl/pipeliner.ex file on the ElixirDrip umbrella project.

We could end here and call it a day; after all, the pipeline DSL is complete and working as expected. But wouldn't it be nice to have some mechanism that would simplify the definition of the pipeline stages as well? Check the next section for some more metaprogramming goodies.

## Streamlining GenStage modules

For now, our pipeline modules (that is, Starter, Encryption, RemoteStorage, and Notifier) are still defining the needed start_link and init/1 functions by themselves. Let's start by streamlining the pipeline producer modules.

## Simpler pipeline producers

For the producer stage, we are currently passing the name and type via the start_link/2 function:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/starter.ex
defmodule ElixirDrip.Storage.Pipeline.Starter do
  use GenStage
  require Logger
  alias ElixirDrip.Storage.Workers.QueueWorker

  @queue_polling 5000
```
