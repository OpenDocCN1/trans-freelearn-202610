The expression pattern is purposely excluded here as it will be covered in detail later. The ' ( ' ) code acts as a placeholder for the expression pattern. The REGEXP_EXTRACT function used in this example is described in Tableau's help documentation as follows:

Returns a substring of the given string that matches the capturing group within the regular expression pattern.

Here are a few important considerations to keep in mind when working with this function:

+   - Note that as of the time of writing, the Tableau documentation does not communicate how to ensure that the pattern input section of the function is properly delimited. For this example, be sure to include ' ( ' ) around the pattern input section to avoid a null output.
+   - Nesting within a calculated field that is itself nested within a VizQL query can affect performance (if there are too many levels of nesting/aggregation).
+   - There are numerous regular expression websites that allow you to enter your own code and help you out, so to speak, by providing immediate feedback based on sample data that you provide. http://regexpal.com/ is only one of those sites, so search as desired to find one that meets your needs!

Now, consider the following expression:

```
\(([0-9]{3}))[0-9]{3}-[0-9]{4}
```

In this context, \ indicates that the next character should not be treated as special but as literal. For our example, we are literally looking for an open parenthesis. The [0-9] instance simply declares that we are looking for one or more digits. Alternatively, consider \d to achieve the same results. The {3} instance designates that we are looking for three consecutive digits.

As with the opening parenthesis at the beginning of the pattern, the \ character designates the closing parentheses as a literal. The – character is a literal that specifically looks for a hyphen. The rest of the expression pattern should be decipherable based on the preceding information.

After reviewing this exercise, you may be curious about how to return just the email address. According to http://www.regular-expressions.info/email.html, the regular expression for email addresses adhering to the RFC 5322 standard is as follows:

```
(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9]))\.){3}(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9])|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])
```