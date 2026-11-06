# Elixir applications

Elixir inherits a lot of concepts from Erlang/OTP, and the application's behaviour is one of those concepts. For the Erlang VM, an application is a component that can be started and stopped as a single unit, and is described by an .app file (for example, hello_world.app) that defines, among other things, the Application Module Callback. This is a module that needs to implement a start/2 function that's responsible for kickstarting the application, usually by spawning its top-level supervisor (in the next chapter, you'll learn all about supervisors). In a way, you can think about the start/2 function as the common main entry point on applications developed with other programming languages.

Because we're using Elixir, we don't need to explicitly specify the .app file. The Elixir compiler will find the correct application module callback by looking for a module using the Application behaviour and will then generate the .app file for us.

Let's use mix to generate a sample project with a supervisor to see all these Application concepts applied in practice:

```
$ mix new sample_project --sup
* creating README.md
* creating .gitignore
* creating mix.exs
* creating config
* creating config/config.exs
* creating lib
* creating lib/sample_project.ex
* creating lib/sample_project/application.ex
* creating test
* creating test/test_helper.exs
* creating test/sample_project_test.exs

Your Mix project was created successfully.
You can use "mix" to compile it, test it, and more:

cd sample_project
mix test
```

We just ran a Mix task named new and passed it two arguments, sample_project and --sup. Mix is a tool that comes with Elixir and helps you to create, organize and maintain your Elixir projects. For example, you can use Mix every time you need to fetch the project dependencies or run the application, by running mix <task> <arguments>. Since dependencies can also add their own Mix tasks, you will be able to run them in the exact same way.