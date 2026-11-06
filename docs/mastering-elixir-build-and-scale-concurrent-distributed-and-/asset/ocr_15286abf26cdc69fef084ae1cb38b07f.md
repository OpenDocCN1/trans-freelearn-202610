In the same umbrella application, you can see the changes in the application.ex file that defines the application callback module. The formatter changed the indentation of the part of the code that starts the database repository supervisor. As you can see, the Supervisor.start_link call now spans seven lines, but it is arguably more intelligible than it was before:

```
$ git diff apps/elixir_drip/lib/elixir_drip/application.ex
# ...
  def start(_type, _args) do
    import Supervisor.Spec, warn: false
-   Supervisor.start_link([
-     supervisor(ElixirDrip.Repo, []),
-   ], strategy: :one_for_one, name: ElixirDrip.Supervisor)
+   Supervisor.start_link(
+     [
+       supervisor(ElixirDrip.Repo, []),
+     ],
+     strategy: :one_for_one,
+     name: ElixirDrip.Supervisor
+   )
  end
end
```

These were just some specific examples of the work performed by the formatter. Automatic code formatting also has its role in many other languages (for example, the Go programming language). It aims to make the code easier to write, easier to learn, read and review and also easier to maintain.

Let's end this part by creating a lint Mix alias that runs the formatter and Credo in a single step when we execute mix lint.

![](bbox=[0.12, 0.66, 0.2, 0.75])

To enforce our static analysis rules, you could even create a way to automatically run mix lint. If you're using git, you can create a pre-commit hook that runs the formatter and only allows you to proceed if no files have been changed by the formatter and no issues were reported by Credo.