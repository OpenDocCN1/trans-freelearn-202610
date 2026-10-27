# Advanced Report Configuration

If you notice the area on the right of **Formula** in the **Custom Summary Formula** editor, there are many functions that you can use to enhance your formula; they are categorized as **All, Logical, Math,** and **Summary**. We will not discuss each function available, but let's look at a sample in the next section.

# Using the IF() function

The IF() function is categorized under the Logical function and the syntax for this formula is **IF(logical_test, value_if_true, value_if_false)**.

This function checks whether a condition returns true. If it does, it will return the first value defined; otherwise, it will return the second value, for example, `IF(RowCount > 4, WON:SUM, CLOSED:SUM)`.

This formula will return a total number of WON opportunities if the number of records is more than four rows; otherwise, it will return the total number of CLOSED opportunities. WON and CLOSED are available from the Opportunity report type.

You don't really have to type `WON:SUM` and `CLOSED:SUM` or other fields you would like to use, but from the **Custom Summary Formula** window, navigate to **Summary Fields | <field name> | Sum**.

Once you click on **Sum**, it will automatically populate in the formula; just make sure that you place the cursor in the formula editor.