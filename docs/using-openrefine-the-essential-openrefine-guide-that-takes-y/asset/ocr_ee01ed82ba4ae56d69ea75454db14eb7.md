OpenRefine allows you to try out regular expressions in an easy way. Create a text filter on any column (the **Object Title** column in the Powerhouse Museum dataset is especially interesting) by clicking on its dropdown and choosing **Text filter**. In the newly created filter, tick the **regular expression** checkbox and you're ready to go (refer to the following screenshot). The data will now be filtered according to the expression you enter:

![](bbox=[0.31, 0.256, 0.69, 0.347])

# Character classes

Regular expressions use **character classes** (letters, numbers, whitespace) as an indication for the character you are looking for. For instance, if you are looking for letters or numbers, you can type them directly:

+   - The pattern `Aar` will look for all texts that contain a capital A followed by a small a and r. If the **case sensitive** box is not ticked, the capitalization will not matter.
+   - The pattern `123` finds all texts that contain this number. Note that texts with the number 41235 are also matched, since `123` is a textual part of that.
+   - As you can see, this does not differ from regular text matching yet. With regular expressions, we can also say that we can expect any letter or any number. We can give a **set of characters** from which can be chosen by surrounding the alternatives with square brackets `[]`. We can indicate a **character range** in those brackets by separating the limits with a hyphen.
+   - The pattern `[0123456789]` matches all texts that contain a number.
+   - The pattern `[0-9]` does the same, only it is more condense. It says everything between 0 and 9 is fine.
+   - Similarly, the pattern `[a-z]` matches all lowercase letters (and also uppercase if the case-sensitive option is not used). `[A-Z]` matches uppercase letters and `[a-zA-Z]` matches all letters, regardless of the case-sensitive option.
+   - If you want to look for numbers and letters, use `[0-9a-zA-Z]`. Read this as "every character between 0 and 9, between a and z, or between A and Z".
+   - As in the earlier examples, we can put several matchers next to each other. `analy[sz]e` will match both American (analyze) and British (analyse) spellings.