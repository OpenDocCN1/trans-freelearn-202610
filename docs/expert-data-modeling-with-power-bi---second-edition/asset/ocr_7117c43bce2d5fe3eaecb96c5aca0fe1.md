```
SimpleFunction = (DateValue as date) as date =>
Date.AddDays(DateValue, 1)
```

We can invoke the preceding function as follows:

```
SimpleFunction(#date(2020,1,1))
```

The result of invoking the function is 2/01/2020.

We can define a custom function as an inline custom function and invoke it within a single query in the Advanced Editor. The following image shows how we use the preceding code to define SimpleFunction as an inline custom function and invoke the function within the same query:

![](bbox=[0.175, 0.395, 0.825, 0.63])

Figure 3.41: Defining and invoking inline custom functions

There are cases where custom functions can help with automating manual and time-consuming development tasks. The following real-world scenario shows how we can save development time using custom functions.

We are tasked to build a data model on top of 100 tables with the following characteristics:

+   - Each table has between 20 and 150 columns.