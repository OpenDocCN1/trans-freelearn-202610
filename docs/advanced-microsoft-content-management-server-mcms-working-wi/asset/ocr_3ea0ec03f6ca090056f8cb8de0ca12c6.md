Note that the channel will not be permanently created until a call to `CommitAll()` is made.

> > Although we only set selected properties of the new channel, you could easily expand the interface to include all writeable properties of the channel object.

When you are done, save and build the project.

# Validating Names

MCMS applies the following rules when naming an object:

+   - The name may only contain the characters:
+     - A-Z a-z 0-9 (alphanumeric only)
+     - _ (underscore)
+     - - (hyphen)
+     - (space)
+     - () (opening and closing parenthesis)
+     - . (period)
+   - The name cannot end with a period.
+   - The name cannot contain two consecutive periods.

To ensure that all names given to channels, postings, template galleries, templates, and resources are valid, we will define a custom method named `ValidateMCMSObjectName()`. The method accepts the name of the object as an input parameter and returns a Boolean. A return value of `true` means that the name provided by the user meets all requirements by MCMS and is valid.

Since we will be adding dialogs to CMS Explorer that create all kinds of MCMS objects, we will add the `ValidateMCMSObjectName()` method to a class file, `Utility.cs`. In this way, we can re-use the method across all web forms within the project.

+   1. Add a class file to the project. Name the file `Utility.cs`.
+   2. Above the namespace declaration, add `System.Text.RegularExpressions`.

```
using System;
using System.Text.RegularExpressions;
namespace CMSExplorer
{
    - - - code continues - - -
}
```

+   3. Directly below the `Utility()` constructor, add the `ValidateMCMSObjectName()` method:

```
public static bool ValidateMCMSObjectName(string name)
{
    bool isValid;

    // test for other characters as the ones in the list
    Regex regEx1 = new Regex(@"[^A-Z0-9 _\-()\.]",
                             RegexOptions.IgnoreCase);
```