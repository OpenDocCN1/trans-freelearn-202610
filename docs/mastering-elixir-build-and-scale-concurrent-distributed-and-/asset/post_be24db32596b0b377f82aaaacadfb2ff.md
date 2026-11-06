
The only way we changed the defchrono/2 macro (besides cleaning up those debug IO.puts/1 calls we had) was to call a new run_and_measure/3 function that also receives the time unit to use. We then passed the @time_unit value to the new run_and_measure/3 implementation that you can find as follows:

```
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  def run_and_measure(time_unit, to_run, to_measure) do
    {time_µs, result} = :timer.tc(to_measure)

    {units, measured_time} = convert_time(time_unit, time_µs)
    IO.puts "Took #{measured_time} #{units} to run #{to_run}"

    result
  end

  defp convert_time(:hrs, time), do: {"hrs", time/(1_000_000*60*60)}
  defp convert_time(:mins, time), do: {"mins", time/(1_000_000*60)}
  defp convert_time(:secs, time), do: {"secs", time/1_000_000}
  defp convert_time(:millisecs, time), do: {"ms", time/1000}
  defp convert_time(:microsecs, time), do: {"µs", time}
  defp convert_time(_, time), do: {"µs", time}
end
```

By passing the time unit via the use mechanism, we are able to measure, using a variety of units, how long the MeasuredModule function calls take. If the :unit entry isn't passed as a use argument in the first place, the @time_unit would be passed to the run_and_measure/3 function as nil, and we would default to showing the time in microseconds (notice the last catch-all convert_time/2 function):

```
iex> c "./examples/measured_module.exs"
[MeasuredModule]

iex> MeasuredModule.slow_square
Took 2.001091 secs to run MeasuredModule.slow_square/1
9

iex> MeasuredModule.slow_times(9, 34)
Took 2.001472 secs to run MeasuredModule.slow_times/2
306
```

As you may have guessed by now, the importance of the use macro is paramount. Everywhere you look, be it Elixir itself or many third-party libraries, you will find the use macro as the common entry point to other modules. Given its pervasiveness, it may strike you as a standard way of injecting code into the caller module.

However, if you want to only define some new functions on the caller module, the `import/1` macro is more than enough.

Remember that the module calling `import ElixirDrip.Chronometer` will be able to call all the public macros and functions defined in the `Chronometer` module, without the need to specify the module first. You can even argue that to achieve the same outcome as we did with the previous `use` example, we could have changed the `defchrono/2` macro to also receive the `time_unit` as an argument, besides the function definition and the body of the measured function. Consider the following macro, which receives the `time_unit` parameter and doesn't change anything else:

```
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  defmacro defchrono_with_units(time_unit, function_definition,
  do: body) do
    {function, args} = Macro.decompose_call(function_definition)
    arity = length(args)

    quote bind_quoted: [
      function_definition: Macro.escape(function_definition),
      body: Macro.escape(body),
      function: function,
      arity: arity,
      time_unit: time_unit
    ] do
      def unquote(function_definition) do
        signature =
          pretty_signature(__MODULE__, unquote(function),
          unquote(arity))

        run_and_measure(unquote(time_unit), signature, fn ->
          unquote(body) end)
      end
    end
  end
end
```

By using the `defchrono_with_units/3` macro, we wouldn't need the `use` mechanism because we wouldn't need to register a module attribute at all in the first place, and the `import` construct would be more than enough for our needs:

```
iex> defmodule SlowMath do
...> import ElixirDrip.Chronometer
...> defchrono_with_units :mins, slow_sum(x, y) do
```

```
...> Process.sleep(1500)
... > IO.puts("slow sum: #{x + y}")
... > x+y
... > end
... > end
```

```
{:module, SlowMath,
<<70, 79, 82, 49, 0, 0, 6, 248, 66, 69, 65, 77, 65, 116, 85, 56, 0,
0, 1, 37,
0, 0, 0, 27, 8, 69, 108, 105, 120, 105, 114, 46, 90, 8, 95, 95,
105, 110,
102, 111, 95, 95, 9, 102, 117, 110, 99, ...>>, {:slow_sum, 2}}
```

```
iex> SlowMath.slow_sum(33, 44)
slow sum: 77
Took 0.02501955 mins to run SlowMath.slow_sum/2
77
```

During our previous macro study, we started by directly importing the Chronometer module to the MeasuredModule, and in the last iteration we injected the import Chronometer statement with the help of the __using__/1 macro. In contrast with how public functions work, macros can't be used until you explicitly say you want to use them. Besides the import/1 macro, you can use the require/2 macro to access all the public macros of a module. However, this approach still expects that we prefix every macro call with the respective module. If we used require instead of import in our example, we would have to define all the MeasuredModule function calls with ElixirDrip.Chronometer.defchrono/2.

This is also the reason, even after doing require Logger, you still need to prefix your debug/1 macro calls with Logger:

```
iex> Logger.debug "hi there"
** (CompileError) iex:2: you must require Logger before invoking the macro Logger.debug/1
(elixir) src/elixir_dispatch.erl:97:
:elixir_dispatch.dispatch_require/6
iex> require Logger
Logger
iex> debug "still problematic"
** (CompileError) iex:3: undefined function debug/1

iex> Logger.debug "finally working"
08:28:53.657 [debug] finally working
:ok
```

We will now continue our trip into the world of macros. On the next stop, we'll have the opportunity to apply what we've learned up to this point and create a domain-specific language to help us define GenStage Pipelines. Let's get started!

## Creating a domain-specific language

You may recall the chapter where we used GenStage to implement our upload and download pipelines. This enabled us to define each step role, and the GenStage behaviour would handle the subscription and message-passing logic between steps, ensuring that no step would get bogged with too many events.

We now want to improve on what we accomplished previously, by streamlining the pipeline and steps definition. To make this happen, we will create our very own Domain-Specific Language (DSL) to help define GenStage-based pipelines, with the help of some interesting macro tools. This will allow us to remove the boilerplate from our pipeline definitions and improve their expressiveness.

![](img/c636f4cfb07f25f1db535995e50706ed_264_0.png)

Given that, with macros, we are writing code that will generate code for us, it is a good tactic to, at least in the beginning, check the final generated code whenever you change the code-generation logic. Remember that Macro.to_string/1 gives you the pretty string representation of any quoted expression, including the AST you're returning from your __using__/1 macro.

Let's start by looking at the end result, so that we know what we are aiming for. Starting with a draft of how we want our pipeline definition gives us a clearer picture of what we need to implement:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/supervisors/
streamlined_upload_pipeline.ex
defmodule ElixirDrip.Storage.Supervisors.Upload.StreamlinedPipeline do
  alias ElixirDrip.Pipeliners
  alias ElixirDrip.Storage.Pipeline.{
    Starter,
    Encryption,
    RemoteStorage,
    Notifier
  }

  use Pipeliner,
    name: :upload_pipeline, min_demand: 1, max_demand: 10, count: 1

  start Starter, args: [:upload]
```

```
step RemoteStorage
step Encryption
finish Notifier
end
```

On this streamlined upload pipeline, we set the default `:min_demand`, `:max_demand` and `:count` options, which will be respected by every pipeline step, if they don't explicitly override them. Optionally, for each step, we can pass arguments that will be used to initialize the state of the GenStage process. We do exactly this on the Starter stage, to indicate from which queue it should fetch the upload tasks to create and emit the corresponding events. The other steps don't need any initialization arguments at all.

Now go back to the ElixirDrip.Storage.Supervisors.Upload.Pipeline and compare our streamlined version to the original upload pipeline. This version is arguably clearer, showing the sequence of stages without the additional statements for the subscription options. As we will see later, the subscription options are being inferred from the relative order of the pipeline stages.

Our plan is as follows:
- We will start by setting the default pipeline options as module attributes.
- Then, we'll collect a list with every stage of the pipeline.
- Afterward, we'll define the worker specs for each stage, so we can supervise each stage under the pipeline supervisor. It's here that we'll also create the logic to generate the subscription options for all the ProducerConsumer and Consumer steps.
- The last step will be to define the start_link/0 and init/1 functions of the pipeline, given that our pipelines are in fact regular supervisors.
- In the end, we will also take the opportunity to streamline our Producer, ProducerConsumer, and Consumer module definitions.

Let's get right into it!

## Registering module attributes

Let's start by setting the default pipeline options. We want to register them as @default_min_demand, @default_max_demand, and @default_count on the caller module. If they aren't passed via the use parameters, we will set them as 1, 10, and 1, respectively.
