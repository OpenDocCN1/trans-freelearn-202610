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