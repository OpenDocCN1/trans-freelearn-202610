```
{:ok, #PID<0.685.0>}

iex> Cache.put("7", "something else")
{:ok, #PID<0.687.0>}

iex> :global.registered_names()
[{:cache, "5"}, {:cache, "7"}]
```

Since a register monitors the registered PIDs, when a registered process exits, the register updates its state accordingly. Let's terminate the {:cache, "5"} process and see what happens:

```
iex> :global.registered_names()
[{:cache, "5"}, {:cache, "7"}]

iex> cache5 = :global.whereis_name({:cache, "5"})
#PID<0.685.0>

iex> Process.exit(cache5, :some_reason)
true

iex> Process.alive?(cache5)
false

iex> :global.registered_names()
[{:cache, "7"}]

iex> :global.whereis_name({:cache, "5"})
:undefined
```

If we want to replace the use of :global by Registry for the CacheWorker processes, we would have to start a named registry instance, let's call it ElixirDrip.Registry, and then we would have to change the {:global, {:cache, <media_id>}} to {:via, Registry, {ElixirDrip.Registry, {:cache, <media_id>}}}. This :via tuple informs the GenServer.start_link/3 function how it should register the soon-to-be spawned process. In our case, it should register the process PID on an existing process named ElixirDrip.Registry, using the Registry module, which implements the register_name/2 function.

![](bbox=[0.13, 0.78, 0.2, 0.87])

Process registers should implement four functions, register_name/2, unregister_name/1, whereis_name/1, and send/2, in fact mimicking what the :global Erlang module does. All of this (and much more) can be found in the Erlang documentation (http://erlang.org/doc/man/gen_server.html#start_link-3).