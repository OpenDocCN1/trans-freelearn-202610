As we can observe from the previous snippet, we now get the return value of the store function, a {:ok, :upload_enqueued, %Media{}} tuple.

Now for the last step of any tracing session: stopping all traces. To accomplish that, we just need to call the following function:

```
iex> :dbg.stop_clear
:ok
```

This is a very simple step. Nevertheless, it's a very important one, as skipping its execution may cause some unpleasant surprises.

Throughout this section, we've learned several techniques to trace our application as needed, depending on the task at hand. Note that these techniques don't have to be used in isolation; it's when combining them that obtain better insights about what's happening with our

Lastly, we want to point out that the tools we've described in this chapter aren't the only useful tools to profile/trace applications. Particularly, we want to mention the XProf (https://github.com/Appliscale/xprof) and Tracer (https://github.com/gabiz/tracer) libraries. We've chosen to demonstrate how to use the :dbg module because we think that it helps to better understand how tracing works on the Erlang VM. However, in your day-to-day work you may resort to using higher-level abstractions, such as the two libraries we've just mentioned. Let's now wrap up this chapter and recap the most important concepts we've explored in it.

# Summary

Throughout this chapter, we analyzed different ways of understanding what's happening under the hood of an Elixir project running in its production environment. Permanently feeding your dashboards with collected metrics allows you to quickly assess your application's current state.

With Prometheus and Grafana, you can then take a deep dive into your metrics data and understand behaviors and patterns that may fly under the radar at first sight. We also learned how to emit custom Prometheus metrics by enriching our Elixir code with instrumentation calls, when the metrics provided out of the box by the Prometheus suite of libraries weren't enough.