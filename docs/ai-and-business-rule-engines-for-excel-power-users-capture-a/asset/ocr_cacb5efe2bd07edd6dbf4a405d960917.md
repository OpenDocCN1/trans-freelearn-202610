We set the decision node to the Context type. While we’ll see another way to use contexts later in this chapter, this example allows us to specify step-by-step instructions, manipulating the list(s) at each step. Each step is a literal expression:

+   1. We create `BaseList` using the FEEL syntax rather than a graphical format – `[30,10,20]`.
+   2. We create another list, `AddedList`, combining `BaseList` with the number we passed in as **Some Input**.
+   3. We create a third list, `SortedList`, and sort our list in it. Note how the `sort()` function takes named parameters and allows us to specify how we want to sort, which makes it very powerful.
+   4. Our final line specifies the value we want to return, (`SortedList`). Changing this value will allow us to view the other lists we’ve created. Clearing this cell (right-click to get the menu option to do this) will return all three lists we have created.

In the example shown in *Figure 9.2*, we pass in a value of 25, which gets added to the list and sorted, giving an output list of `[10,20,25,30]`. Note the syntax using the square brackets and commas, which allows you to write a list expression in a very compact form.

# More on manipulating lists using FEEL expressions

Now that you’ve seen the basics of lists and their syntax, the rest of the functions should make a lot more sense. We’ll do a quick run-through – most likely, you just need to know that these functions exist. When you need to use them in detail, you can then find out the exact parameters using the FEEL guide we mentioned earlier:

+   - We’ve seen the `sort` and `append` functions in the example. `reverse` and `remove` are the opposite of these.
+   - Max, Mean, Min, Sum, and Count work pretty much as you’d expect.
+   - `distinct values()` removes duplicates from a list – for example, `distinct values(["One", "One", "Two"])` will return a shorter list with no duplicates, `["One", "Two"]`.
+   - `index of()` returns where the item can be found in the list. For example, `index of(["One", "Two", "Three", "Four"], "Four")` will return the number 4. `contains` is similar but returns `true` or `false` depending on whether the list contains the value or not.

Lists can also contain lists – while this is a simple concept, any example we could give would get complicated very quickly! We’ll just mention that `flatten()` will combine all the sublists into one big list. `concatenate()` is similar but gives you slightly more control over the order of the list that is created. `sublist()` allows you to chop up this list into shorter lists again.