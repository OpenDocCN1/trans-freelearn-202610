What makes contexts more powerful is that it is easy to add values. For example, `context put (myContext, "Raising Agent", 1)` would add that item to our ingredient list.

While the full list of context functions is well documented in the FEEL handbook, it is worth highlighting `context.merge()`, which allows us to combine contexts, and `context.get entries ()`, which returns a list of all the values in context.

There are several other graphical ways of creating contexts that are worth exploring.

# Graphical contexts

Graphical contexts allow us to define contexts in a more visual format, which is useful for less experienced colleagues to understand what our models do. Remember we came across the other use of contexts in Figure 9.2. In that example, we used a FEEL expression context to generate values step by step, with a `result` statement to pass back only the final value.

If we delete or clear that final `result` statement, all of the generated values will be returned as a set of `key:value` pairs. We’ll demonstrate this with an example that we’ll build out in the rest of this chapter – the Bill of Materials decision model. This is shown in Figure 9.4 and can be downloaded as `09 Bill of Materials.dmn` from the book’s GitHub repository.

![](bbox=[0.26, 0.47, 0.77, 0.76])

Figure 9.4 – Our Bill of Materials decision service

This decision model has two inputs: `Qty` (Number) and `Product` (String). The third node, `Product Details`, we saw earlier – it’s a decision node of the `Relation` type, holding information on the ingredients for each product.