We pattern match on the storage value and only do something if the process was using a :dets table for storage. This callback won't be called if the process ends abruptly (for example, it receives a :brutal_kill from the supervisor or an exit signal other than :normal), thus, in some cases the DETS table may not be closed. If this happens, the next time the table is opened, it will be repaired by :dets. This reparation process is a costly operation and can take a while to repair large tables, so it's always better to properly close the table.

With the FlexibleSearchCacheWorker, we can now quickly change from an in-memory ETS table to a disk-based version of it, and vice-versa. This flexibility was only possible because the :ets and :dets modules have the same contract for most of the commonly used functions.

The next level of data storage provided by Erlang is Mnesia. We won't talk about it in this book, but you should know it gives you an abstraction closer to what one expects from a database, including a nicer query language and atomic transactions. It can be transparently replicated through different nodes, allowing you to query data without knowing where the data actually lives. It can also be persisted both in memory and in disk. Think of it as the child prodigy of the ETS and DETS modules (it is actually backed by them).

We will now end our ETS and DETS analysis and dive into the amazing world of process registers. Without them, we would have to know the PIDs of all the processes our applications spawn, and life would be much harder.

# Registry

When we started our GenServer-based CacheWorker, you may recall that we used {:global, {:cache, <media_id>}} as the value for the :name option so that we could use the {:cache, <media_id>} tuple as its name. Behind the scenes, our cache processes were being registered using the :global Erlang module, which is started with every running application and serves as a single distributed process register when we have more than one node connected. At this stage, we are still running a single node, thus, as far as we can tell, we could be using Registry, an Elixir process register introduced in version 1.4.

A process register lets us associate a name to a given process. Let's query the :global registry after spawning two cache workers:

```
iex> alias ElixirDrip.Storage.Supervisors.CacheSupervisor, as: Cache
ElixirDrip.Storage.Supervisors.CacheSupervisor

iex> Cache.put("5", "something")
```