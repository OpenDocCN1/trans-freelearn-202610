
## Advanced Report Configuration

Let's look at the report result using the preceding formula:

![](img/72d01bea5a9ccf02f9452aab7505d9de_181_0.png)

Let's analyze the preceding report; **Check Count** is a summary formula:

- For **OPPORTUNITY OWNER Alvin Lee**, since there are four record, the formula will return the number of closed opportunities for **Alvin Lee**, which is 2. In this exercise one opportunity is **Won** and one opportunity is **Lost**.
- For **OPPORTUNITY OWNER Johan Yu**, there are five records; the formula will return the number of won opportunities for **Johan Yu**, which is 4.
- For the **GRAND TOTAL**, the total records are nine; the formula will return the number of won opportunities in total, which is 5: one opportunity from **Alvin Lee** and four opportunities from **Johan Yu**.

## Using the MAX() function

The MAX function is categorized as the Math function; the syntax for this formula is MAX(number,number,...).

This function returns the greatest of all arguments, for example,

```
MAX(Opportunity.Projected_Revenue__c:SUM, AMOUNT:SUM).
```

The same is true with the IF() function explained earlier; you can just point and click to get the preceding formula populated. In this sample, we use the opportunity report type, and the MAX() function returns the maximum value by comparing the sum of a custom field called PROJECTED REVENUE with the sum of AMOUNT. A sample report is shown as follows:

![](img/72d01bea5a9ccf02f9452aab7505d9de_182_0.png)

Let's analyze the preceding report; MAX AMOUNT is a summary formula:

- We set the MAX AMOUNT summary field to display only at the grouping level, and that's why it does not show in GRAND TOTAL
- Because this is a summary formula, it will not compare the **AMOUNT** value with the **PROJECTED REVENUE** value in record level but only compare it in the **Summary**
- For **OPPORTUNITY OWNER** Alvin Lee, **AMOUNT** is greater than **PROJECTED REVENUE**, so **MAX AMOUNT** is equal to **AMOUNT**
- For **OPPORTUNITY OWNER** Johan Yu, **PROJECTED REVENUE** is greater than **AMOUNT**, so **MAX AMOUNT** is equal to **PROJECTED REVENUE**.

## Function formulas

There are two functions in Salesforce available only in the summary formula and considered the most powerful report formula. They are categorized under the Summary functions: `PARENTGROUPVAL` and `PREVGROUPVAL`.

### The PARENTGROUPVAL() function

The `PARENTGROUPVAL()` function returns the value of a specified grouping; the grouping level is higher than the formula display level.

The formula syntax is different depending on the report format:

- In the **Summary** and **Joined** reports: *PARENTGROUPVAL(summary_field, grouping_level)*
- In the **Matrix** report: *PARENTGROUPVAL(summary_field, parent_row_grouping, parent_column_grouping)*

### Using the PARENTGROUPVAL() function in Summary report

For the **Summary** report, the `PARENTGROUPVAL()` function is not applicable when you select the formula displayed as **All summary levels** or **Grand summary only**. The only formulas displayed as field grouping can use this function.

Next, we need to populate the value for both parameters **Summary Fields** and grouping level. Let's start with the grouping level parameter; I'll tell you why I start with grouping level instead of the **Summary Fields** parameter.

## Advanced Report Configuration

Let's look at a hands-on exercise to populate the parameters; in this exercise, we have a **Summary** report with two levels of field grouping; the first level is **OPPORTUNITY OWNER** and the second level is **TYPE**:

1. Make sure that you select the formula to be displayed as the field grouping, not all summary or grand summary; because the report has two levels of field grouping, we will have two options. They are **Grouping 1: Opportunity Owner** and **Grouping 2: Type**; let’s select **Grouping 2: Type** for this exercise.
2. Place your mouse in the **Formula** editor and select the **PARENTGROUPVAL** function from the dropdown.
3. If you have only one level of field grouping in the summary report, the parent grouping_level parameter is always **GRAND_SUMMARY**, while if you have more than one level as in this exercise (two levels of grouping), we will have the option of **GRAND_SUMMARY** and field name of the first level grouping for the parent grouping level, which is Opportunity Owner in our exercise.
4. The field name for the first-level grouping in the parent grouping level will be enabled when the formula to be displayed is selected with the second-level grouping. In summary, the availability of the parent grouping level is one level higher than the selected formula to be displayed. Compare the following screenshots; the first one is when **Grouping 2: Type** is selected:

![](img/72d01bea5a9ccf02f9452aab7505d9de_184_0.png)

Compare this with the following screenshot. When **Grouping 1: Opportunity Owner** is selected, **Opportunity Owner** becomes disabled in the parent grouping level:

![](img/72d01bea5a9ccf02f9452aab7505d9de_185_0.png)

5. Let's select **Grouping 2: Type** for this exercise and select **Opportunity Owner** as the parent grouping level from the radio button.
6. Click on the **< Insert** button, and you will get the formula added in our sample here: **PARENTGROUPVAL(summary_field, FULL_NAME)**.
7. Delete the **summary_field** text and change it with the correct field summary. To populate the **summary_field** parameter, place your cursor before the comma, click on **Summary Fields**, select the field name you would like to use (for example, **Amount**), and select the action (for example, **Sum**). Now your formula should be *PARENTGROUPVAL(AMOUNT:SUM, FULL_NAME)*.

Here is the final formula. Look at the arrows in the following screenshot for the relationship between the grouping levels:

![](img/72d01bea5a9ccf02f9452aab7505d9de_186_0.png)

Next, let's create a new Summary report:

1. Group the report by CLOSE DATE (Group Dates by Calendar Month) and then group by Opportunity Owner.
2. Add a summary formula and name it Amount by Close Date; add this formula and display at Opportunity Owner (Grouping 2) with formula as PARENTGROUPVAL(AMOUNT:SUM, CLOSE_DATE):.
3. Add the summary formula created to the report.
4. Add the Amount field and summarize it with Sum. This is shown in the following screenshot:

## Advanced Report Configuration

| CLOSE DATE | OPPORTUNITY OWNER | OPPORTUNITY NAME | AMOUNT Sum | AMOUNT BY CLOSE DATE |
|---|---|---|---|---:|---:|
| June 2016 (3 records) | Alvin Lee (1 record) | Pyramid Emergency Generators | $100,000.00 | |
|  |  | Subtotal | $100,000.00 | $265,000 |
|  | Johan Yu (2 records) | Element Stage 1A | $85,000.00 | |
|  |  | Element Stage 1B | $80,000.00 | |
|  |  | Subtotal | $165,000.00 | $265,000 |
| Subtotal |  |  | $265,000.00 | - |
| July 2016 (3 records) | Alvin Lee (1 record) | Element Phase IIa | $38,000.00 | |
|  |  | Subtotal | $38,000.00 | $213,000 |
|  | Johan Yu (2 records) | Project Motor Head | $100,000.00 | |
|  |  | Project Motor Links in Kentica | $75,000.00 | |
|  |  | Subtotal | $175,000.00 | $213,000 |
| Subtotal |  |  | $213,000.00 | - |
| GRAND TOTAL (6 RECORDS) |  |  | $478,000.00 | - |

Let's analyze the report from the screenshot:

- **In June 2016**, Total Amount is **$265,000.00**. We see that this value is the same as the value from the summary formula; this means that the value of the summary formula from one level above, which is subtotal of Close Date.
- **In July 2016**, Total Amount is **$213,000.00**, which is the same value in the summary formula.
- The value of the formula will be available in **Opportunity Owner** (which is level 2) grouping in the report, and the parent grouping level is **CLOSE_DATE**.

Next, let's create another similar formula in the same report:

1. Name is **Amount by Grand Total**.
2. Select and display the summary formula at **Opportunity Owner (Grouping 2)**.
3. Select the parent grouping level as **Grand summary**.
4. Formula is *PARENTGROUPVAL(AMOUNT:SUM, GRAND_SUMMARY)*.

![](img/72d01bea5a9ccf02f9452aab7505d9de_188_0.png)

Let's analyze the preceding screenshot:

- Note that the summary formula value is the same as the value of **GRAND TOTAL**.
- The summary formula is shown in the **Opportunity Owner** grouping, which is the Grouping 2 we configured, and the value is a copy from **GRAND_SUMMARY** (this is two levels above Opportunity Owner in the hierarchy).
- However, if you change the formula to show in **Close Date** (Grouping 1), the value would be the same from **GRAND TOTAL** (this is one level above Close Date). Refer to the following screenshot:
