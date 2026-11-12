Now that you have reached this point, you should be familiar with manipulating data in `Dictionary` variables. The steps provided in this section enable you to easily initialize, assign, and retrieve values from `Dictionary` variables to use in real-world use cases. The use of `Dictionary` variables is similar to `List` and `Array` variables. The only difference is that `Dictionary` variables' values are accessed through a unique key instead of the memory location.

# Understanding pattern-based data extraction with regexes

We discussed extracting, converting, and performing several other manipulations on different data types during this chapter. Up to now, we discussed pretty straightforward situations where you had the exact value you need to manipulate. However, there are scenarios where you need to extract specific values based on a pattern from larger datasets, such as text paragraphs. We need to use unique functionalities such as regexes to extract only the value we need in such scenarios.

The importance of regexes is that it enables the user to match, locate, and extract values based on a specific pattern. For example, if you have a text paragraph that contains account numbers and email addresses, you can use regexes to extract only email addresses from the input string.

However, building regexes can be challenging. UiPath provides a regex builder to simplify the complexities around these expressions. Regexes are commonly used to parse text and for input validation, data scraping, and string manipulation. Hence, regexes are a part of string manipulation. However, we thought of addressing regexes separately as they are quite different from the other manipulation methods that we have covered.

UiPath provides three methods that use regexes along with RegEx Builder. Each method is a separate activity in UiPath that is associated with RegEx Builder. The three methods are listed here:

+   • Matches: Used to search for a specific pattern in an input string and returns all matching values as `IEnumerable<Match>`. You can access values in this type by using a **For Each** activity by setting the **Type Argument** field to `System.Text.RegularExpressions.Match`.
+   
+   • IsMatch: Used to check whether a specific pattern exists in the input string. This function returns a Boolean value based on the result.