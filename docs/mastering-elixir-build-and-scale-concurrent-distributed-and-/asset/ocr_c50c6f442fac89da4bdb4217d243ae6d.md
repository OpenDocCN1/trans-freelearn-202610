```
iex> v(-3)
21
```

There are more things you can do with IEx, such as configuring it or connecting to remote shells. Please refer to the official documentation (at https://hexdocs.pm/iex/IEx.html) for further usage examples of IEx.

# Mix

Mix is the Swiss-army knife of the Elixir tools. It's used to compile and run your application, manage your dependencies, run tests, and even for profiling your code. You can see which Mix tasks are available by typing `mix help` in your terminal. We won't explain its usage here, since the next chapter's purpose is to show you how you can use Mix to create and maintain an Elixir project.

# ExUnit

Elixir comes with a fully fledged unit test framework: ExUnit. We'll use it to write tests for the application built in the course of this book. Chapter 9, Finding Zen Through Testing, is dedicated to testing, where we'll demonstrate how you can write tests for your application that let you sleep at night.

# Erlang interoperability

As we stated at the beginning of this chapter, Elixir targets the Erlang runtime. Elixir is compiled to byte-code that can run on an Erlang VM (or BEAM), and Erlang libraries can be used in your Elixir projects (and vice versa). The philosophy in the Elixir community is to not reinvent the wheel and directly use Erlang libraries when appropriate. The creation of Elixir libraries that simply wrap an underlying Erlang library is discouraged, as you can directly call an Erlang library from your Elixir code.

We can take advantage not only of Erlang libraries, but also of their tooling. For instance, Erlang ships with a tool called Observer, which allows you to monitor your server and provides you with tons of useful information about the Erlang VM, such as the running processes or dynamic charts of load. We'll explore this tool in greater detail in Chapter 11, Keeping an Eye on Your Processes, when we talk about monitoring.