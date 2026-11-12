# The Immediate panel

The **Immediate** panel can be used for checking the data/output of a variable, arguments, or statements at a given point during the debugging process:

```
Immediate
> firstName.Contains("Jane")
< true
> firstName.ToUpper
< "JANE"
> companyName.GetType
< [System.String]
>
```

Figure 13.12 – The Immediate panel

To get the output, type the variable or argument name in the **Immediate** window and press *Enter*. The output doesn't get updated during the debugging process; rather, it keeps the history of previously evaluated statements.

Using the context menu, you can copy the previously evaluated expression or use the **Clear All** option to clear all lines in the panel. Using *Ctrl + spacebar* opens up IntelliPrompt, which helps to select the desired functions or properties associated with the variable or argument:

```
Immediate
> firstName.Contains("Jane")
< true
> firstName.ToUpper
< "JANE"
> companyName.GetType
< [System.String]
CompareTo
Concat(Of ...)
Contains
Contains(Of ...)
ContainsUnmatchedParentheses
CopyTo
Count(Of ...)
>
companyName.co
```

Figure 13.13 – IntelliPrompt using Ctrl + spacebar