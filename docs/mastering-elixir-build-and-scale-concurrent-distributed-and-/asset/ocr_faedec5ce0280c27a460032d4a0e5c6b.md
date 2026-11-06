![](bbox=[0.13, 0.13, 0.2, 0.23])

Every test file on the `test` folder has to end with `_test.exs`. If not, your test cases won't run when you do `mix test`. Mix will only warn you if you have a test file in the `test` folder whose name ends in `_test` but does not have the expected extension; if the name does not match `*_test`, it proceeds without any warning, and your test file simply won't run.

Let's now check how we can define the dependencies of an Elixir project.

# Project dependencies

We already analyzed the purpose of the `mix.exs` file. Its importance is paramount, since the information contained in it defines how the Erlang VM actually runs your Elixir project. However, we didn't examine how Mix actually uses this file to also know the dependencies of our application and how it should fetch and start them.

Mix looks for the application dependencies in the `deps` entry returned by the `project/0` function of the module defined by the `mix.exs` file. Each entry of this list is a tuple, pointing to the application our application depends on.

To help developers find libraries, the Elixir core team created Hex (https://github.com/hexpm/hex), a package manager for the Erlang VM that Mix uses by default. You can find all the public available libraries on hex.pm (https://hex.pm/), and there is also the possibility of uploading your libraries to private repositories hosted by hex.pm.

If you just set the dependency name and version in the `mix.exs` file (that is to say, `{:library, "~> 1.0.1"}`), it will be fetched from hex.pm. If your dependency lives in a private repository, you should add the `organization` option to its dependency entry in the `deps` list.

You can also pass additional options for each dependency. By using the `git` or `path` options, you can fetch the dependency from a Git repository or a local path, respectively. As you will see next, dependency management inside umbrella applications also leverage the same dependencies mechanism: if your dependency lives under the same umbrella project, you can use it by setting the `in_umbrella` option to `true`.

Elixir makes managing your dependencies a breeze: by default, Elixir will add your dependency to the so-called *runtime applications*. This ensures your dependencies are all running when your application starts and are included when you create your application release package.