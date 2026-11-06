If you try to replicate it on your local machine, you may just get insignificant data and never get to the bottom of the issue you're trying to solve.

Elixir, standing on the shoulders of Erlang, has great support for profiling and tracing your applications safely in production. As we’ve said before, a great deal of care is necessary when doing this sort of operation in a live environment. Nevertheless, it's incredibly useful to have such great support from the Erlang VM to carry out profiling and/or tracing in your application.

It's important to note that, contrary to other systems, we won't pry into our production application by setting breakpoints and going step by step to try to analyze the context. All the observations that we will make are indirect and will never stop any process that's running in production. In the first chapter of this book, we saw how to use IEx helpers to set breakpoints from the shell. That's a helpful technique to debug your application when developing it locally, but it's not suitable for inspecting your application in production.

This section is divided into two sub-sections: in the first, we explore the use of one of the profilers built into Mix to understand what's happening with a certain function in our application, whereas in the latter, we use Erlang's `:dbg` module to trace how live requests are handled.

# Profiling with the fprof profiler

We're now going to learn how to use one of the profilers that ships with Mix to analyze the performance of a certain function in our application. The profiler we're going to use is based on Erlang's `fprof` profiler. Mix exposes this profiler through a Mix task, which is how we're going to interact with it.

We're going to profile the `ElixirDrip.Storage.store/4` function, which is responsible for creating a new `Media` entry on the database and triggering an upload task. To simulate that this function is taking way longer than expected to complete, we're going to insert a `Process.sleep/1` call into the `ElixirDrip.Storage.Media.create_initial_changeset` function, which is one of the functions called by the `store` function.

The fascinating and intriguing cases to profile only come to life when an application is deployed in a live environment, is used by actual users, and generates unpredictable workloads. Nonetheless, using a `Process.sleep/1` call allows us to demonstrate the important aspects, which will enable you to carry out your own profiling whenever you have the need.