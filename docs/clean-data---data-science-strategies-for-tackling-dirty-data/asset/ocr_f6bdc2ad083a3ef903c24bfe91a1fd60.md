3. Now, use **Find-Replace** to remove the [ and ] characters from columns A and B, replacing them with nothing. (Highlight these columns before starting find-replace so that you do not accidentally remove symbols throughout the whole sheet.)

Unfortunately, Excel, in its eternal quest to try to help us, turns these into dates formatted in a way we might not like: 9/19/2014. If you want these to go back to the way they were (2014-09-19), select the entire column and use the custom format dialogue to specify the date format as yyyy-mm-dd.

4. The strings in column **F** still have extra space at the beginning. We can strip the extra spaces from the front of each string value using the `trim()` function. Insert a new column to the left of **F**, and apply the `trim()` function, shown as follows:

![](bbox=[0.18, 0.36, 0.83, 0.43])

This is the result of applying the `trim()` function to strip out leading or trailing spaces.

5. We can also apply the `clean()` function to the trimmed text. This will remove any of the first 32 ASCII characters: all non-printing control characters that may have somehow made their way into these channel descriptions. You can apply `clean()` outside `trim()` functions like this: `clean(trim(g1))`.

6. Drag the corner of the F1 box down to apply `clean(trim())` to the rest of the cells in column **F**.

7. Select column **F**, copy it, and use **Paste Special Values Only** in column **F** so that we can delete column **G**.

8. Delete column **G**. Voila, now you have perfectly cleaned data.