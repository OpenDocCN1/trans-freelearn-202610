# External application example: In-cell sort

This example shows how to sort the text contents of one cell. We start by modifying the **text_plain_external.inc.php** script, as mentioned in the above section, to add the **sort** program:

```
$allowed_programs[0] = '/bin/sort';
```

Note that our new program bears the index number 0.

We then add a **TEXT** field and **keywords** to our book table. Finally, we fill in the MIME-related information, entering '0','-r' as the transformation options.

![](bbox=[0.26, 0.33, 0.74, 0.41])

The '0' here refers to the index number for **sort**, and the '-r' is a parameter for **sort**, which makes the program sort in the reverse order.

Next, we **Edit** the row for the book "A hundred years of cinema (volume 1)", entering some keywords in no particular order (as seen in the following screenshot) and hitting **Go** in order to save the changes.

![](bbox=[0.18, 0.52, 0.82, 0.62])