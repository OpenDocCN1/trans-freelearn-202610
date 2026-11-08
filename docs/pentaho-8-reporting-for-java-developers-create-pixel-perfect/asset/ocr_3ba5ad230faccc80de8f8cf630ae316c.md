```
IF([COL1] > 10; "Big"; "Small");
```

This formula uses the IF function with parameters separated by semicolons. The first parameter is a comparison, resulting in true or false. To reference an outside data column or named function, reference the column in brackets, as shown for COL1. If the comparison example evaluates to true, the second parameter in the IF function is returned. To specify a string, use double quotes, shown with "Big" and "Small". If the statement evaluates to false, the third parameter is returned. Also, the term null may be used to denote a null reference.