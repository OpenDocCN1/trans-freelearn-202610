```
    x * x
    end)
end
[MeasuredModule]

iex> MeasuredModule.slow_square()
Took 2000981 µs to run MeasuredModule.slow_square/1
9

iex> MeasuredModule.slow_square(5)
Took 2000324 µs to run MeasuredModule.slow_square/1
25
```

This poses as a valuable lesson that you should follow when using macros. Do as much as possible on the macro context to inject the smallest code chunk possible into the caller context. And when your macro receives arguments, try to think about every possible way those arguments may be passed to cater for all edge cases. Those who use your macro may not be as fluent as you with metaprogramming, and the errors that result from using macros are sometimes cryptic, in contrast to the detailed error messages we came to expect and appreciate from normal Elixir. This is one of the reasons why you should be doubly careful with your macros.

Our defchrono_v4 macro is now working as expected. Let's take one step further and make this work using the bind_quoted option on the quote block. This is the trickiest part of our defchrono macro, due to the different moments when the macro is evaluated.

On this new macro iteration, we'll use for the first time the Macro.escape/1 function. Let's just do a brief detour to understand how it works. You can think about this function as a cousin of the quote/1 macro, since it also returns a quoted expression. However, instead of always returning the quoted representation of the code it receives, it evaluates the code it gets and only then returns its quoted representation. Check the following snippet that illustrates this difference:

```
iex> a = %{this: "is", a: "map"}
%{a: "map", this: "is"}

iex> quoted_a = quote do: a
{:a, [], Elixir}

iex> escaped_a = Macro.escape a
{:%{}, [], [a: "map", this: "is"]}
```