

## Data Validation and Form Controls

2. Type Recommended_for_promotion as shown in Figure 5.16 and then click OK.

![](img/0c7a357df7e57e06b6d9f631c219040c_158_0.png)

Figure 5.16 – The New Name dialog box

> **Tip**
> Names cannot contain spaces, so make sure to place an underscore between each word. Also, make sure to double-check your spelling; otherwise, this technique won’t work. Names are not case-sensitive.

3. Select cell S2:S4 and then choose Formulas | Define Name.
4. Type Not_recommended_for_promotion and then click OK.

Let's now walk through the INDIRECT function.

## The INDIRECT function

Cell C3 of the INDIRECT function worksheet contains the =INDIRECT(B3) formula, as shown in Figure 5.17. Cell C3 will return December when you type F13 in cell B3 and then press Enter. This works the same way as typing =F13, but the difference is that you would have to replace =F13 with =F6 to reference May while INDIRECT allows you to type F6 in cell B3. In this context, INDIRECT converts the text in cell B3 to a valid cell reference.

![](img/0c7a357df7e57e06b6d9f631c219040c_159_0.png)

Figure 5.17 – The INDIRECT function

The INDIRECT function has the following two arguments:

- Ref_text: The text that you want to convert to a cell reference or **Name**.
- A1: An optional argument where you specify TRUE for an **A1 reference** style or FALSE for an **R1C1 reference** style. In our case, we’ll omit this argument because we want to use the default style where we reference cells based on the column letter and row number, meaning **B3**, rather than the **R1C1** reference style of R3C2, meaning third row and second column.

INDIRECT is going to enable us to dynamically reference the Names of the two dependent lists. Now let’s see how to use the SUBSTITUTE function to replace spaces with underscores.

## The SUBSTITUTE function

As shown in Figure 5.18, cell **D4** contains the =SUBSTITUTE(B4," ","_") formula, which replaces any spaces in cell **B4** with underscores.

![](img/0c7a357df7e57e06b6d9f631c219040c_159_1.png)

Figure 5.18 – The SUBSTITUTE function

The SUBSTITUTE function has the following four arguments:

- Text: The text to be searched.
- Old_text: The existing text you wish to remove.
- New_text: The replacement text you wish to use instead.
- Instance_num: This optional argument lets you indicate a specific instance of old_text to replace. The formula in cell D4 omits this argument.

With that background in mind, let’s now create dependent Data Validation lists.

## Assembling the lists

Let’s now create the primary and secondary lists as follows:

1. Click on cell D17 on the Data Validation worksheet and choose Data | Data Validation.
2. Choose List from the Allow field and then enter or select R1 : S1 in the Source field, as shown in Figure 5.19.

![](img/0c7a357df7e57e06b6d9f631c219040c_160_0.png)

Figure 5.19 – Row-based Data Validation list source

3. Complete the Input Message and Error Alert tabs and then click OK.
4. Select cell D18 and choose Data | Data Validation.
5. Choose List from the Allow field, and then enter =INDIRECT(SUBSTITUTE($D$17,"","_")) in the Source field, as shown in Figure 5.20.

![](img/0c7a357df7e57e06b6d9f631c219040c_161_0.png)

Figure 5.20 – INDIRECT/SUBSTITUTE functions

6. Complete the **Input Message** and **Error Alert** tabs and click **OK**.

The choices available in cell **D18** are now contingent upon the choice that you make in cell **D17**.

> Nuance
> An invalid entry can remain in cell **D18** if you set cell **D17** to *Recommended for promotion* and make a selection in **D18** but then change cell **D17** to *Not recommended for promotion* without also updating **D18**. You can enter the =ISNA(MATCH(D17,INDIRECT(SUBSTITUTE(D17," ","_")),0)) formula in a custom **conditional formatting** rule to make cell **D18** change colors if the entry in the cell does not appear on the corresponding list. We discuss **conditional formatting** and the MATCH function in *Chapters 4, Conditional Formatting*, and *10, Lookup and Dynamic Array Functions*, respectively.

Now let’s see how to use the **Date** rule.

## Date

The **Date** rule requires a user to enter a date in cells where the rule is in use. For instance, valid inputs in cell **C20** of the **Data Validation** worksheet are any dates between January 1, 2024 and December 31, 2024. The following steps show you how to recreate this functionality:

1. Select cell **D20** and then choose **Data | Data Validation**.
2. Choose **Date** from the **Allow** list and then choose **Between** from the **Data** list.
3. Enter `1/1/2024` in the **Start Date** field and `12/31/2024` in the **End Date** field.
4. Complete the **Input Message** and **Error Alert** tabs and then click **OK**.

You can also create dynamic validation rules with functions such as TODAY and EOMONTH.

## The TODAY function

The `=TODAY()` formula in cell **C3** of the **TODAY function** worksheet returns today’s date. This function does not have any arguments, nor does the **NOW** function in cell **C4**, which returns the current date and time. TODAY and NOW are often interchangeable unless you’re comparing a date to the result of the NOW function – the decimal portion that NOW returns that represents the current time can cause the comparison to fail.

Now let’s see how to use EOMONTH.

## The EOMONTH function

The `=EOMONTH(C4,0)` formula in cell **D4** on the **EOMONTH function** worksheet returns **12/31/2024** since cell **C4** contains **12/1/2024**. EOMONTH is short for _end of month_, and has the following two arguments:

- **Start_date**: A static or calculated date.
- **Months**: The number of months in the future or past for which you want to calculate a month-end date. For instance, use `0` for the current month, `1` for the last day of the following month, `-1` for the last day of the previous month, and so on.

Now let’s use TODAY and EOMONTH to create a dynamic **Data Validation** rule.

## Formula-based Date rule

Cell **C21** of the **Data Validation** worksheet requires an input that is greater than or equal to the first day of the following month. The following steps show you how to recreate this functionality:

1. Click on cell **D21** and choose **Data | Data Validation**.
2. Choose **Date** from the **Allow** List and **Greater than** from the **Data** list.
3. Enter the `=EOMONTH(TODAY(),0)` formula in the **Start Date** field, as shown in **Figure 5.21**.
4. Complete the **Input Message** and **Error Alert** tabs and then click **OK**.

![](img/0c7a357df7e57e06b6d9f631c219040c_163_0.png)

Figure 5.21 – Dynamic Date rule

Nuance
Pay particular attention to whether you want to use **greater than** or **greater than or equal to** when using date inputs. For instance, if you used `=EOMONTH(TODAY(),0)+1` with the **greater than** rule, then the user would not be able to enter the first day of a month as a valid choice. Conversely, **greater than or equal to** would work in that case, because adding 1 to the EOMONTH calculation returns the first day of the following month.

This brings us to the **Time** rule.

## Time

Cell **C23** of the **Data Validation** worksheet requires an input of a time that is between `8:00 AM` and `5:00 PM` (`08:00` to `17:00`). The following steps show you how to recreate this functionality:

1. Select cell **D23** and then choose **Data | Data Validation**.
2. Choose **Time** from the **Allow** list and accept the default of **Between** in the **Data** list.
3. Enter 8:00 AM (or 08:00) in the **Start time** field and 5:00 PM (or 17:00) in the **End time** field, as shown in Figure 5.22.

![](img/0c7a357df7e57e06b6d9f631c219040c_164_0.png)

Figure 5.22 – Time restriction rule

Tip
Excel recognizes inputs such as 8 a and 5 p as valid time inputs as well. Inputs such as 8 and 17 are not valid.

4. Complete the **Input Message** and **Error Alert** tabs, and then click **OK**.

Nuance
Time values from 12:00 PM and onward are tricky. If you type 3:00 in cell C16, Excel will interpret the input as 3:00 AM and will display the error alert. When completing the **Input Message** and **Error Alert** tabs, remind the user to include AM or PM in their time entries to avoid frustration.

This brings us to the **Text length** rule, which controls the number of characters that can appear in a cell.

