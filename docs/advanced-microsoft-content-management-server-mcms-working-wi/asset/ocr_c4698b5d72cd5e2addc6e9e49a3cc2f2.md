```
```csharp
// test for "." at the end
Regex regEx2 = new Regex(@"[\\.]$", RegexOptions.None);

// test for double "."
Regex regEx3 = new Regex(@"\\.{2}", RegexOptions.None);

Match match1 = regEx1.Match(name);
Match match2 = regEx2.Match(name);
Match match3 = regEx3.Match(name);

// valid = no invalid chars, no double dot, and no dot at the end
isValid = !match1.Success && !match2.Success && !match3.Success;

return isValid;
}
```
```

The ValidateMCMsObjectName() method uses three regular expressions to check the validity of the name.

The first regular expression looks like this:

```
```text
[^A-Z0-9 _\\ (.-]
```
```

It’s a pattern that finds all characters in the name that are not alphanumeric, spaces, underscores, opening and closing parenthesis, periods and hyphens (note that the backslash character is used as an escape character), satisfying the first condition for a valid name.

The second regular expression tests to see if the name ends with a period:

```
```text
[\\.]$
```
```

Again, the backslash has been used as an escape character. The period between the square brackets indicates that we are looking for periods. The dollar sign instructs the pattern to match the end of the string.

The third and last regular expressions tests for double periods in the string:

```
```text
\\.{2}
```
```

As before, the backslash is used as an escape character. The number 2, surrounded by curly braces, indicates that we are looking for double occurrences of the period.

Finally, we run all three regular expressions. As long as there are no invalid characters, periods at the end of the name, or consecutive periods, the name is considered to be valid.

To use the ValidateMCMsObjectName() method within a dialog, we will simply call it from within the code. Here's how it's done for the Create Channel dialog. Add the following code to the btnCreate_Click() event handler:

```
```csharp
private void btnCreate_Click(object sender, System.EventArgs e)
{
    - - - code continues - - -

    try
    {
        - - - code continues - - -

        // get the name of the channel
        string Name = txtName.Text.Trim();

        // check to see if the name is valid
        if (!Utility.ValidateMCMsObjectName(Name))
```
```