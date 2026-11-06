Elixir can make this match succeed by binding the x variable to the 3 term. On the third line, we're again matching the left and right sides, and it succeeds because x has the 3 value, so both sides are equal. On the next line, we're trying to match the 2 literal to the x variable. Elixir can't find a way to make this match work, which results in an error being raised. It's important to point out that binding to variables only happens when they are on the left-hand side of the = operator—when they're at the right-hand side, the variable is simply replaced by its value.

From this snippet, we can also notice that we always have a return value—even when doing a pattern match. This is the expected behavior, since everything in Elixir is an expression–there are no statements. Every operation you can do will always return a value, whether you're printing something to the console or making an HTTP request. While you can achieve the same things with expressions and statements, always having a return value is very useful because you can chain functions together and define the program flow according to the values being returned. In the case of pattern matching, when it is successful, we always get back the term that was matched on the right-hand side.

The match operator is not confined to bind variables to simple values–it's actually a very powerful operator that is able to destructure complex data types (and make your code ridiculously simple to read). We will now show how you can use this operator on several of the types we've presented in the previous section, while also demonstrating other aspects of the pattern matching process.

# Pattern matching on tuples

The following snippet shows how pattern matching can be done on tuples:

```
iex> {number, representation} = {3.1415, "π"}
{3.1415, "π"}
iex> number
3.1415
iex> representation
"π"
```

The process here is the same as we have described in the preceding snippet. By setting the {number, description} pattern on the left-hand side, we're stating that we expect a tuple with two values—again, if that's not the case, a MatchError will be raised. In this case, the match succeeds, and we can see that the variables number and representation are bound to the expected values.