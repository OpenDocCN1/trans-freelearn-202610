from the executors memory asynchronously. However, this method provides an overload where the user can run unpersist synchronously as well.

```
public void unpersist(boolean blocking)
```

Therefore, unpersist can be done synchronously as follows:

```
broadcastVar.unpersist(true);
```

The `unpersist()` method removes the broadcast variable from the executors memory. However, it does not remove it from the driver's memory. If the broadcast variable is used again in the program after `unpersist()` is called, it will be resent to the executors.

+   • Destroy: The `destroy` method removes all the data and metadata related to the broadcast variable from executors and drivers asynchronously. Once, the broadcast variable has been destroyed, it cannot be called again. Any attempt to do so will throw an exception. It can be done as follows:

```
broadcastVar.destroy();
```

Similarly to the `unpersist` method, the `destroy` method also provides an overload, so it can be called synchronously as follows:

```
broadcastVar.destroy(true);
```

It is recommended to unpersist a broadcast variable before destroying it as it gets removed from the executors memory asynchronously first and does not block the Spark program execution.

Once a broadcast variable has been destroyed and garbage collected, all the data and metadata related to it is cleaned by `contextCleaner`.

`ContextCleaner` is a Spark service which is responsible for cleaning up RDDs,