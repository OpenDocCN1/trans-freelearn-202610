+   6. In the generated expression, replace the TO BE REPLACED string with the following expression:

```
each if Text.Length([Description]) > 30 then
[Description] else ""
```

+   7. Replace REPLACING WITH with the following expression:

```
each Text.Start([Description], 30) & "..."
```

+   8. Click the Submit (✔) button.

The overall expression must look like this:

```
= Table.ReplaceValue(#"Renamed Columns", each if Text.Length([Description]) > 30 then [Description] else "",each Text.Start([Description], 30) & "...",Replacer.ReplaceValue,{"Description"})
```

The preceding code block returns the following output:

![](bbox=[0.17, 0.52, 0.83, 0.69])

Figure 5.48: Cutting off the values in the Description column that are longer than 30 characters

Apply the changes and save the file as a new file: Chapter 5, Common Data Preparation Steps, Replaced Values.pbix.

Replacing values is a widespread transformation step with many use cases, such as replacing values based on another column, values from a previous