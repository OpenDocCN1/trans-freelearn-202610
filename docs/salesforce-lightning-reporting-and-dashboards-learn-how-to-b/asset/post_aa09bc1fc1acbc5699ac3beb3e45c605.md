
## Advanced Report Configuration

- 1. Go to the **Reports** tab and click on the **New Report...** button.
- 2. Select the **Accounts** Report Type and click on the plus sign under the **Accounts & Contacts** category.
- 3. Click on the **Create** button to continue.
- 4. Change **Show** to **All accounts**.
- 5. Change the **Date Field Range** to **All Time**.
- 6. Change the format to **Summary** and click on **Remove All Columns**.
- 7. Add **Account Name** and **Type** to the report from the **Fields** panel.
- 8. Drag **Account Owner** to the grouping area.
- 9. Double-click on **Add Formula** in the left-hand side panel under **Fields**. This is shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_172_0.png)

- 10. Enter **Column Name** as `Total Open Opportunities`.
- 11. Enter **Description** as `Total Amount of Open Opportunities`.
- 12. Select **Format** as **Currency** and **Decimal Places** as **2**.

## Advanced Report Configuration

- 13. Select **Grand summary only**.
- 14. In the **Formula** field, navigate to **Summary Fields | Total Amount of Open Opportunities | Sum**. Consider the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_173_0.png)

- 15. The formula will be automatically filled with **Account.Total_Amount_of_Open_Opportunities__c:SUM**.
- 16. Click on the **OK** button and note the **Total Open Opportunities** option added in the formula and in the report column.
- 17. Note that the formula summary field is always added in the last column of the report and you cannot move it to the left.
- 18. Save the report as **Open Opportunities**.
- 19. Click on **Run Report**.

Here is the report result:

![](img/72d01bea5a9ccf02f9452aab7505d9de_174_0.png)

## Advanced Report Configuration

20. Now let's change the formula by hovering the mouse over the **Total Open Opportunities** formula and then clicking on the pencil icon.

![](img/72d01bea5a9ccf02f9452aab7505d9de_175_0.png)

To edit or delete the formula, when the report is in the report builder, hover the mouse over the formula and click on the pencil icon or the trash bin icon; this is the same with the bucket field.

21. Change from **Grand summary only** to **All summary levels**. This is shown in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_175_1.png)

22. Click on the **OK** button and then click on **Run Report**.

23. Take a look at the changes in the following screenshot; the Subtotal value for Account Owner is now populated:

# REPORT
Open Opportunities

Total Records
17
Total Open Opportunities
$1,250,000.00

| ACCOUNT OWNER | ACCOUNT NAME | TYPE | TOTAL OPEN OPPORTUNITIES |
|---|---|---|---|
| Alvin Lee (4 records) | Accurate Glass | Customer - Channel | |
|  | Element Industry Ltd | Prospect | |
|  | United Oil & Gas, UK | Customer - Direct | |
|  | United Oil & Gas Corp. | Customer - Direct | |
| Subtotal |  |  | $395,000.00 |
| Johan Yu (13 records) | Burlington Textiles Corp of America | Customer - Direct | |
|  | Dickenson plc | Customer - Channel | |
|  | Edge Communications | Customer - Direct | |
|  | Express Logistics and Transport | Customer - Channel | |
|  | GenePoint | Customer - Channel | |
|  | Grand Hotels & Resorts Ltd | Customer - Direct | |
|  | King Inc | Customer - Direct | |
|  | PMA Manufacturing | Prospect | |
|  | Pyramid Construction Inc. | Customer - Channel | |
|  | Quality Materials | Customer - Channel | |
|  | Super Force Inc | Customer - Direct | |
|  | United Oil & Gas, Singapore | Customer - Direct | |
|  | University of Arizona | Customer - Direct | |
| Subtotal |  |  | $855,000.00 |
| GRAND TOTAL (17 RECORDS) |  |  | $1,250,000.00 |

But since the **TOTAL AMOUNT OF OPEN OPPORTUNITIES** is a roll-up summary field in Account, we can just add the field to the report and summarize it, and we will get a similar report result:

| REPORT Open Opportunities |
| Total Records | Total Total Amount of Open Opportunities | Total Open Opportunities |
| 17 | $1,250,000.00 | $1,250,000.00 |
| ACCOUNT OWNER | ACCOUNT NAME | TYPE | TOTAL AMOUNT OF OPEN OPPORTUNITIES Sum | TOTAL OPEN OPPORTUNITIES |
| Alvin Lee (4 records) | Accurate Glass | Customer - Channel | $0.00 | |
|  | Element Industry Ltd | Prospect | $0.00 | |
|  | United Oil & Gas, UK | Customer - Direct | $0.00 | |
|  | United Oil & Gas Corp. | Customer - Direct | $395,000.00 | |
| Subtotal |  |  | $395,000.00 | $395,000.00 |
| Johan Yu (13 records) | Burlington Textiles Corp of America | Customer - Direct | $0.00 | |
|  | Dickenson plc | Customer - Channel | $15,000.00 | |
|  | Edge Communications | Customer - Direct | $35,000.00 | |
|  | Express Logistics and Transport | Customer - Channel | $200,000.00 | |
|  | GenePoint | Customer - Channel | $60,000.00 | |
|  | Grand Hotels & Resorts Ltd | Customer - Direct | $265,000.00 | |
|  | King Inc | Customer - Direct | $0.00 | |
|  | PMA Manufacturing | Prospect | $0.00 | |
|  | Pyramid Construction Inc. | Customer - Channel | $100,000.00 | |
|  | Quality Materials | Customer - Channel | $0.00 | |
|  | Super Force Inc | Customer - Direct | $80,000.00 | |
|  | United Oil & Gas, Singapore | Customer - Direct | $0.00 | |
|  | University of Arizona | Customer - Direct | $100,000.00 | |
| Subtotal |  |  | $855,000.00 | $855,000.00 |
| GRAND TOTAL (17 RECORDS) |  |  | $1,250,000.00 | $1,250,000.00 |

In this case, it is not necessary to create a summary formula unless you need to hide the amount of each Opportunity and just get the total. The sample in this hands-on does not show the real power of the report formula summary; except for the normal Summarize field, we cannot configure it, and it will always show at all levels. Let's look at a better usage of summary formula in the next hands-on exercise.

## Hands-on exercise for working with a summary formula

Let's consider a use case: in a **Summary** report, add **Total Amount of Closed Opportunities** in all summary levels. Assuming that roll-up summary fields in Account from Opportunity called **Total Amount of Opportunities** and **Total Amount of Open Opportunities** have been created and the detail for **Total Amount of Open Opportunities** field has been discussed in the previous hands-on, here is the roll-up summary field detail for **Total Amount of Opportunities**:

![](img/72d01bea5a9ccf02f9452aab7505d9de_178_0.png)

- 1. Create a new custom summary formula field similar to the one we created in the hands-on exercise earlier and name it `Total_Closed_Opportunities`, with this formula: `Account.Total_Amount_of_Opportunities__c:SUM - Account.Total_Amount_of_Open_Opportunities__c:SUM`

2. For the sake of clarifying for this exercise, let's add both Total Amounts of Opportunities and Total Amount of Open Opportunities roll-up summary fields to the report and then summarize them with Sum. Consider the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_179_0.png)

3. In the preceding report, Total Amount of Closed Opportunity (summary formula) is equal to Total Amount of Opportunity (roll-up summary field) minus Total Amount of Open Opportunity (roll-up summary field). We use the Subtract operator between summary formulas, and we can also use this technique for the other formulas:

![](img/72d01bea5a9ccf02f9452aab7505d9de_179_1.png)

## Advanced Report Configuration

If you notice the area on the right of **Formula** in the **Custom Summary Formula** editor, there are many functions that you can use to enhance your formula; they are categorized as **All, Logical, Math,** and **Summary**. We will not discuss each function available, but let's look at a sample in the next section.

## Using the IF() function

The IF() function is categorized under the Logical function and the syntax for this formula is **IF(logical_test, value_if_true, value_if_false)**.

This function checks whether a condition returns true. If it does, it will return the first value defined; otherwise, it will return the second value, for example, `IF(RowCount > 4, WON:SUM, CLOSED:SUM)`.

This formula will return a total number of WON opportunities if the number of records is more than four rows; otherwise, it will return the total number of CLOSED opportunities. WON and CLOSED are available from the Opportunity report type.

You don't really have to type `WON:SUM` and `CLOSED:SUM` or other fields you would like to use, but from the **Custom Summary Formula** window, navigate to **Summary Fields | <field name> | Sum**.

Once you click on **Sum**, it will automatically populate in the formula; just make sure that you place the cursor in the formula editor.
