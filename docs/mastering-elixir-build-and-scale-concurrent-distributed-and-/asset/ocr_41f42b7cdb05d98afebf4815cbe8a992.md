```
end
end
end
```

On the start_link/0 function, we simply start the supervisor with the name we previously obtained. On the init/1 function, we fetch the pipeline steps, convert them to worker specs, and then initialize the supervisor with the mentioned worker specs.

Notice that most of the functions used for the DSL aren't injected on the caller module. We opt instead to call the functions that live on the ElixirDrip.Pipelinermodule (the caller module can access those because it imports the Pipelinermodule). This is a good practice that aims to avoid cluttering the caller module with functions that are only needed in compile time for macro expansion.

![](bbox=[0.12, 0.36, 0.2, 0.44])

To see the full code of the ElixirDrip.Pipelinermodule, check the apps/elixir_drip/lib/elixir_drip/dsl/pipelin er.ex file on the ElixirDrip umbrella project.

We could end here and call it a day; after all, the pipeline DSL is complete and working as expected. But wouldn't it be nice to have some mechanism that would simplify the definition of the pipeline stages as well? Check the next section for some more metaprogramming goodies.

# Streamlining GenStage modules

For now, our pipeline modules (that is, Starter, Encryption, RemoteStorage, and Notifier) are still defining the needed start_link and init/1 functions by themselves. Let's start by streamlining the pipeline producer modules.

# Simpler pipeline producers

For the producer stage, we are currently passing the name and type via the start_link/2 function:

```
$ cat apps/elixir_drip/lib/elixir_drip/storage/pipeline/starter.ex
defmodule ElixirDrip.Storage.Pipeline.Starter do
  use GenStage
  require Logger
  alias ElixirDrip.Storage.Workers.QueueWorker

  @queue_polling 5000
```