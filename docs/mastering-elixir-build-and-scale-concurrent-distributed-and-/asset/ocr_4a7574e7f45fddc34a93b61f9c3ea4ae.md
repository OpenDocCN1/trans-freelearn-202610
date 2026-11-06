We will now continue our trip into the world of macros. On the next stop, we'll have the opportunity to apply what we've learned up to this point and create a domain-specific language to help us define GenStage Pipelines. Let's get started!

# Creating a domain-specific language

You may recall the chapter where we used GenStage to implement our upload and download pipelines. This enabled us to define each step role, and the GenStage behaviour would handle the subscription and message-passing logic between steps, ensuring that no step would get boggled with too many events.

We now want to improve on what we accomplished previously, by streamlining the pipeline and steps definition. To make this happen, we will create our very own Domain-Specific Language (DSL) to help define GenStage-based pipelines, with the help of some interesting macro tools. This will allow us to remove the boilerplate from our pipeline definitions and improve their expressiveness.

![](bbox=[0.12, 0.5, 0.2, 0.62])

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