# Tracing with the :dbg module

Having seen how to profile the modules and functions of our application, let's now explore how to do tracing in a live environment. While in the previous section, we had to provide the data for the function we wanted to profile as input, in this section we'll follow a different approach. We will tell the VM the functions that we want to see traced, and then we'll observe the live calls that our users are making in production. This is really interesting because we're analyzing the behavior of our application with real data, which usually allows us to draw better conclusions. Moreover, as we discussed previously, there are certain bugs that only manifest themselves in a live environment. In such cases, we need to be able to tap into the live requests and try to figure out what's happening with them.

To carry out the tracing, we'll be using Erlang's :dbg module. There are some lower-level constructs to carry out tracing experiments, such as the :erlang.trace function. We think that dealing directly with these constructs brings no added value over just using :dbg, which provides a useful abstraction layer to carry out tracing experiments without having to worry about too many details. Moreover, we feel that using :dbg directly is already dangerous enough, since if misused, it can cause great damage to your production environment. The lower-level commands are usually more error-prone, and as such using them directly increases the likelihood of inadvertent side effects in production.

Let's check how a tracing experiment usually takes place. We begin by calling the following command in our IEx session:

```
iex> :dbg.tracer
{:ok, #PID<0.661.0>}
```

Calling the tracer function will spawn a new process, called a tracer process, which will be responsible for collecting the events associated with this tracing experiment. This process will receive messages in its mailbox according to the tracing patterns we define. In the following examples, we'll just print the tracing events to the console, but it's possible to configure :dbg to save the trace output to a file, which is handy when you want to analyze it later on.

To choose which functions we want to trace we'll use the :dbg.tp function:

```
iex> :dbg.tp(ElixirDrip.Storage, :store, 4, [])
{:ok, [{:matched, :elixir_drip@10.40.0.41, 1}]}
```

Using the tp function, we specify the module, function, and arity we want to trace. We may omit the arity of the function, which will result in tracing all the arities of that function name. In the same way, we may omit the function name, which will result in all of the functions of that module being traced.