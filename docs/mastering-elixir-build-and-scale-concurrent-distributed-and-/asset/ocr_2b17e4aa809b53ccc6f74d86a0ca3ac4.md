```
iex> MeasuredModuleExpanded.slow_square
Took 2000317 µs to run MeasuredModuleExpanded.slow_square/1
9
```

We covered a lot of new concepts and techniques up to now, and it may seem daunting at first. But be rest assured that independently of what we do with macros, the only magic happening is that we are injecting code into the caller module. The injected code needs to be in a specific quoted format, and that's it.

Let's now check how we can pass options to macros and use this technique to choose which time unit should be used to display the execution time reported by our defchrono/2 macro.

# Using the use and __using__/1 macros

Our objective is now to choose the time unit that will be used to time our functions. To achieve this we will employ the use macro, which you have probably seen a few times now (the CacheWorker module previously implemented by us used GenServer). Instead of importing the ElixirDrip.Chronometer module, we are now using it:

```
$ cat examples/measured_module.exs
defmodule MeasuredModule do
  use ElixirDrip.Chronometer, unit: :secs

  defchrono slow_times(x, y) do
    Process.sleep(2000)
    x * y
  end

  defchrono slow_square(x \\ 3) do
    Process.sleep(2000)
    x * x
  end
end
```

When the compiler finds the use statement on the MeasuredModule, it will run the __using__/1 macro of the ElixirDrip.Chronometer module. You will see this strategy a lot because it provides a standard way for the used module to be invoked, and most of the times injects whatever it needs into the caller module. Another extremely useful feature of use is that it lets us pass arguments to the invoked __using__/1 macro. In the preceding example, we are passing a keyword list with a single :unit entry.