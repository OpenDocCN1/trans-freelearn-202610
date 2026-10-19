
- Custom data types allow us to build containers to hold our data, using the basic building blocks (such as text, number, and Boolean). We also looked at constraints on data, to limit the values that can be used.
- Lists may have looked familiar (since they are like a range of cells). We’ll introduce more advanced functions to manipulate them that stretch this concept. We’ll also cover functions to test ranges (think of a list but with sequential values – for example, `[1..10]`).
- Related to lists are **contexts** (also known as **maps** or **relations**), which are like a list of key-value pairs – more on those soon, since while they are easy to use, the idea may be new to you as an Excel Power User.
- We saw `if... then` expressions for branching and will soon introduce loops to act on list data.
- We covered previously all the different node types in DMN (text, decision nodes, including decision tables, input data, and functions), and will return to invocation to better organize your decision models.

Before we (re)start, there are a couple of FEEL language basics worth remembering:

- FEEL is designed to be **side effect-free** – evaluating an expression shouldn’t change the values passed into that expression. This is different from (for example) Visual Basic code, which might update the value or color of a cell in Excel.
- FEEL is designed to be **human-readable**, even allowing spaces in variable and function names. If you’ve used another computer language (such as JavaScript), you may find this strange.
- Because of this, it’s worth remembering that FEEL will evaluate any text not in quotes as a variable name. `"Friday"` (with quotes) will match that exact text, while `Friday` (no quotes) will make KIE complain if there is no variable defined with that name. Both (with or without quotes) are **FEEL literals** – an expression that KIE can evaluate to a value.

We’ll start exploring the FEEL language with lists – a very powerful but easy-to-use concept. But since lists work slightly differently than Excel, it’s worth looking at a few more in-depth examples.

## Dynamic lists, contexts, and relations

You might remember in *Chapter 4* that we graphically created `List` using a decision node of the `List` type. Look at *Figure 4.25* if you want to refresh your memory – it was a great way to clearly show the health issues we were looking for. But lists are much more dynamic than that. We already said that we could use FEEL expressions anywhere – and that includes within lists. For example, the list in *Figure 9.1* is a valid syntax. This example can be downloaded from the sample site (`https://github.com/PacktPublishing/AI-and-Business-Rules-for-Excel-Power-Users`) as `c-09-dynamic-lists.dmn`.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_238_0.png)

Figure 9.1 – Generating our list values dynamically

We’ve manipulated the figure so we can show the main diagram, our list values, and the input/output in one picture. You will have to press the **Edit** node and **Run** button on the screen to view similar results.

> **Date and time functions in FEEL**
> 
> As you’d expect, the FEEL reference lists many functions for date and time handling. We used the `now()` function in this example, and most date functions behave like their equivalents in Excel.
> 
> The FEEL guide gives a good overview of dates – even if it is split across several sections of the guide. One thing worth highlighting is the `.` syntax – for example, the expression `(2023,12,20).day` function will return 20 with similar options for `.year`, `.month`, and so on.
> 
> You’ll notice that support for international dates is not as extensive as in Excel – for example, it is always listed in year/month/day order – date("2023-12-20") is December 20, 2023. Notice how we created the same date but using a string instead of numbers. There are also similar constructor options for **time**, **date and time**, and **duration**.

Let’s look at the example in *Figure 9.1* from left to right:

- We have a very simple decision model – just one input called **Some Input**. This feeds the value into a decision node of the `list` type (we’ve called it `Dynamic List`).
- Our list has four values. The first of these is static text. The second is today’s date. The third is our input value, converted into uppercase. And the final item is a number that we calculate.
- On the right-hand side are the inputs and outputs. We pass in a simple value (`hello`) and get a list of four values back – we can tell it’s List as they are numbered **0-3**. While this is common for scripting languages, it is different from Excel, which starts counting rows at 1.

It’s worth downloading the example and playing around with it to explore it further.

## Conversion functions

You'll notice our example uses a string() function to convert both the current date into a string and Some Input passed into a string – FEEL doesn't convert values automatically in the way Excel does.

There are equivalent functions for converting into numbers and dates, with more information on how to use them in the FEEL reference guide.

While the items in this list are generated dynamically, it will always have the same four items in the same order. We can do something more, well, dynamic with lists. Take a look at Figure 9.2, also downloadable as 09 Step by Step Context.dmn.

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_239_0.png)

Figure 9.2 – A more dynamic list

The decision model for this sample is again simple – an input node feeding into the decision node – so we haven't shown it. We do edit the properties of the Some Input node to specify that only numbers are allowed, which means we don't have to convert in our functions. If you are trying to create this example yourself, it is worth noting that each of the boxes in the right-hand column of the table is a literal expression, and that the names should be consistent. For example the name Some Input has a space in it, so all expressions referring to it should also have that space.

We set the decision node to the Context type. While we’ll see another way to use contexts later in this chapter, this example allows us to specify step-by-step instructions, manipulating the list(s) at each step. Each step is a literal expression:

- 1. We create `BaseList` using the FEEL syntax rather than a graphical format – `[30,10,20]`.
- 2. We create another list, `AddedList`, combining `BaseList` with the number we passed in as **Some Input**.
- 3. We create a third list, `SortedList`, and sort our list in it. Note how the `sort()` function takes named parameters and allows us to specify how we want to sort, which makes it very powerful.
- 4. Our final line specifies the value we want to return, (`SortedList`). Changing this value will allow us to view the other lists we’ve created. Clearing this cell (right-click to get the menu option to do this) will return all three lists we have created.

In the example shown in *Figure 9.2*, we pass in a value of 25, which gets added to the list and sorted, giving an output list of `[10,20,25,30]`. Note the syntax using the square brackets and commas, which allows you to write a list expression in a very compact form.

## More on manipulating lists using FEEL expressions

Now that you’ve seen the basics of lists and their syntax, the rest of the functions should make a lot more sense. We’ll do a quick run-through – most likely, you just need to know that these functions exist. When you need to use them in detail, you can then find out the exact parameters using the FEEL guide we mentioned earlier:

- We’ve seen the `sort` and `append` functions in the example. `reverse` and `remove` are the opposite of these.
- Max, Mean, Min, Sum, and Count work pretty much as you’d expect.
- `distinct values()` removes duplicates from a list – for example, `distinct values(["One", "One", "Two"])` will return a shorter list with no duplicates, `["One", "Two"]`.
- `index of()` returns where the item can be found in the list. For example, `index of(["One", "Two", "Three", "Four"], "Four")` will return the number 4. `contains` is similar but returns `true` or `false` depending on whether the list contains the value or not.

Lists can also contain lists – while this is a simple concept, any example we could give would get complicated very quickly! We’ll just mention that `flatten()` will combine all the sublists into one big list. `concatenate()` is similar but gives you slightly more control over the order of the list that is created. `sublist()` allows you to chop up this list into shorter lists again.
