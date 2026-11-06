We will now analyze how the defchrono_v5 macro works with the bind_quoted option. As we have already seen, the bind_quoted option let's you access the variables defined by it on the quote block because it defines for us as many assignments as there are entries on the bind_quoted keyword list. Its usage also disables unquote/1 by default, so those unquote/1 calls (for the function_definition, function, arity, and body) will appear in their AST form on the quoted representation returned by the macro:

```
```elixir
$ cat apps/elixir_drip/lib/elixir_drip/chronometer.ex
defmodule ElixirDrip.Chronometer do
  # ...
  defmacro defchrono_v5(function_definition, do: body) do
    {function, args} = Macro.decompose_call(function_definition)
    arity = length(args)
    IO.puts "On defchrono_v5"
    IO.inspect function_definition
    IO.puts "Escaped function_definition"
    IO.inspect Macro.escape(function_definition)
    IO.puts "Function"
    IO.inspect function
    IO.puts "Arguments"
    IO.inspect args

    ast_to_return = quote bind_quoted: [
      function_definition: Macro.escape(function_definition),
      body: Macro.escape(body),
      function: function,
      arity: arity
    ] do
      def unquote(function_definition) do
        signature =
          Chronometer.pretty_signature(__MODULE__, unquote(function),
          unquote(arity))

        Chronometer.run_and_measure(signature, fn -> unquote(body) end)
      end
    end

    IO.puts "Returning"
    ast_to_return
    |> Macro.to_string()
    |> IO.puts()

    ast_to_return
  end
end
```
```