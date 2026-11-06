The modules we'll be showing in this example aren't the ones used in the final application. For instance, the Encryption module will be using GenStage (further explained in the next chapter), which would clutter our example. In fact, the Encryption module is so simplified that's merely a process running in a loop, waiting for messages, and printing the ones it receives to the terminal. This simplification will allows us to focus solely on the supervisor, which is the focus for this section. Note that you can always see all the source code by downloading it from the book's website.

To plug a module into the supervisor's behaviour, we only need to implement one callback: init/1. This callback will provide the specification of the processes that are started by this supervisor, along with some configuration. To interact with supervisors, we use the Supervisor module provided by Elixir. Let's see the code for the upload pipeline supervisor:

```
$ cat examples/upload_pipeline_supervisor.ex
defmodule ElixirDrip.Storage.Supervisors.Upload.Pipeline do
  use Supervisor
  require Logger
  alias ElixirDrip.Storage.Pipeline.Encryption

  def start_link() do
    Supervisor.start_link(__MODULE__, :ok)
  end

  def init(:ok) do
    Logger.debug("#{inspect(self())} Starting the Upload Pipeline Supervisor module...")

    Supervisor.init(
      [worker(Encryption, [], restart: :transient)],
      strategy: :one_for_one
    )
  end
end
```

Beginning with the init function, we can see that it receives one argument. In this case, we're pattern matching to :ok, as we won't be using it, but this argument could be used to pass a list of options from the caller process. Besides some logging that is helpful for debugging, the only thing this function does is call the Supervisor.init/2, which will start the children we pass as the argument. As we've stated earlier, we need to provide to the supervisor the specification of the processes that will be started under this supervisor–the list of child processes. This is the first argument passed to Supervisor.init/2.