Once you configure the **Write Line** activity, your workflow should look similar to this:

![](bbox=[0.142, 0.138, 0.858, 0.672])

Figure 7.2 - String.Contains function example

The `String.Contains` function gives you `True` as the result if the searched string is available. If you want the function to return `False` when the string exists (simply by reverting the result), you can introduce the `Not` keyword into the expression. In the previously described example, the expression would look like this after introducing the `Not` keyword:

```
Not String.Concat(DisplayText, FirstName, LastName).
Contains("John")
```