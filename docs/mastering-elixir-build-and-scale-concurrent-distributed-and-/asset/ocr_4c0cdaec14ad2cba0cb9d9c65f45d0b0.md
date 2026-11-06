+   - If you want the caller module to be able to override some of the functions you're injecting, the `@behaviour`, `@impl`, and the `defoverridable/1` macro can help you to safely implement adopted behaviours and allow the caller module to redefine some of those default implementations.

You can now go out and explore, having all these new tools at your disposal.
Metaprogramming begs you to tinker with it and to try new ways of applying it. At this stage, you aren't confined to the standard building blocks you're offered at the entrance, but you are now able to create your very own building blocks from scratch!

# Summary

We accomplished a lot throughout this chapter. Let's look back at the main topics that we covered:

+   - The Elixir abstract syntax tree consists of a tree-like data structure composed of nested three-element tuples representing your code. This data structure can be manipulated using the `quote/2` and `unquote/1` constructs and is also called a *quoted expression*.
+   - By default, Elixir enforces a macro hygiene rule that doesn't allow us to impinge into the caller module, unless we specifically want to.
+   - Macros are functions whose arguments are quoted before reaching the function body and have to return valid quoted expressions as well. Macros are expanded right before compile time.
+   - The `use` and `__using__/1` macros let the macro module inject code into the caller module. We used these constructs to register a `@time_unit` module attribute in the caller module, so we could control the time units used by the `defchrono/2` macro.
+   - We created a domain-specific language to streamline the definition of parallel GenStage pipelines, and we also improved the pipeline stage definition, only requiring the `handle_demand/2` and `handle_events/2` callbacks to be implemented.

Here, we have understood how metaprogramming works. In the following chapter, we will delve into Ecto and how it helps us to persist and query our application data. Given the heavy metaprogramming usage by Ecto, you will feel right at home!