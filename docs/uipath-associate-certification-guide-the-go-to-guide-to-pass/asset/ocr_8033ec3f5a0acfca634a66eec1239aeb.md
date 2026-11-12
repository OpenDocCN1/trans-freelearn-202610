10. Execute the program, and you will see the following output from the last **Message Box** activity:

![](bbox=[0.177, 0.152, 0.789, 0.332])

Figure 7.4 – String.Format function output

The `String.Format` option can be helpful in scenarios where you have to replace dynamic parts of a string with values retrieved during execution. One good example is updating the body of a standard email message with the name of the person addressed. So, let's look at the `Replace` function.

# Replace function

The `Replace` function enables the user to replace all occurrences of a specified substring in the primary string. Similar to the `Contains` function, this function is also case-sensitive. It uses the following expression to replace text:

```
VarName.Replace(<String to Replace>, <New Value>)
```

The `Replace` expression requires two parameters: the string to be replaced and the new text.

Let's apply this function to the same workflow and try replacing some values, as follows:

+   1. Open the previously built workflow in UiPath Studio.

+   2. Drag and drop an **Assign** activity after the last **Message Box** activity into the workflow.

+   3. Configure the **To** segment of the **Assign** activity by creating a new string variable under the name `CleansedValue`.

+   4. Configure the **Value** segment of the **Assign** activity by providing the following code:

```
DisplayResult.Replace("Hello...", "")
```