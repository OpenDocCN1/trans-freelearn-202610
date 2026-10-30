
## MyFirstDataset

706 rows

Learn to use the fields panel

Search fields...

Show Hidden Fields (0)

Show Field API Names

## Dimensions

+   - Account ID
+   - AccountId.BillingCity
+   - AccountId.BillingCountry
+   - AccountId.BillingState
+   - AccountId.Id
+   - AccountId.Industry
+   - AccountId.Name
+   - AccountId.OwnerId
+   - AccountId.OwnerId.Id
+   - AccountId.OwnerId.Name
+   - Contact ID
+   - ContactId.Id
+   - ContactId.MailingCity
+   - ContactId.MailingCountry
+   - ContactId.MailingState
+   - ContactId.Name
+   - Lead Source
+   - Name
+   - Opportunity ID
+   - Owner ID

## My First Lens

Datasets

Add Dataset

MyFirstDataset

DATA

FILTERS

Bar Length

Sum of Amount

Bars

OwnerId.Name

Trellis

Unused Fields

Query Limit

Default

Edit

Close

Save

Figure 6.11 – The Fields panel

The options that are available for each field are as follows:

+   - **Rename**: To rename the field label.
+   - **Hide**: To hide the field.
+   - **Edit Values** (dimensions or dates only): A field label can be edited here, and a default color can be added for each value.
+   - **Format Numbers** (measures only): Numbers can be formatted to suit your requirements, such as currency rounded or percent.
+   - **Copy API Name**: Copies the field API name to your clipboard.

> **Note Regarding Changes Made in the Fields Panel**
> It is important to note that any changes that are made here will apply to the dataset wherever it is used – these are global changes.

With the knowledge you've gained thus far, it's time to get your hands dirty with a practical exercise.

## Hands-on exercise

Follow these steps in your TCRM Analytics Studio:

+   1. In the TCRM Analytics Studio, open **My First Analytics App** and click on the **Cases** dataset to create a new lens.
+   2. Save the lens as **Service Data Analysis** in **My First Analytics App**.
+   3. To begin with, create a time combo chart with **Avg of Case Duration** and **Count of Rows** trended over **Closed Date (Year-Month)**.
+   4. **Save** your lens.

The lens should look something like this:

![](img/06379e43783b9ddfee45e9da5192efe1_169_0.png)

Figure 6.12 – Hands-on exercise starting point (case data)

Change the chart type to combo (instead of time combo), then modify your data so that the x axis is the industry and answer these questions:

+   - What account industry has the most cases?
+   - What account industry has the highest average case duration?
+   - Set some custom colors and modify the value names for some Account.Industry fields.
+   - Use the drill-down button to slice and dice several industries by other dimensions, such as Billing Country, Priority, and Status.
+   - Experiment with the following chart types and understand where each is useful: Line, Combo, Timeline, Flat Gauge, Polar Gauge, Donut, Matrix, Waterfall, and Scatter Plot.
+   - What is the best chart for looking at the correlation of case priority and status? Build this lens and save it as a copy (cloned lens).
+   - Use the trellis functionality and think where it could best be used.

What did you learn about by diving into your data with lenses? The following are some example questions:

+   - What limitations did you hit while working only with clicks, not code?
+   - Are there any chart types that you are unsure of using, or think are not of much use in the real world?
+   - Can you create charts of exactly the right look and feel, or is customization limited in lenses?

Now that you're confident with Chart Mode, it's time to use a flexible, no-code analysis tool in TRCM lenses called Table Mode.

## Analyzing your data using Table Mode

In this section, you will learn how to use one of the more flexible and powerful tools in TCRM – tables.

First, let's look at the values table.

## Values table

The values table is used to look at record values. Open My First Lens and change to Table Mode – Values Table. The lens will change to a table view, like this:

![](img/06379e43783b9ddfee45e9da5192efe1_170_0.png)

Figure 6.13 – Initial values table view

Follow these steps to create a new table view:

+   - Select **Clone in New Tab** and save as **My First Table View**.
+   - Filter the data by **Stage = Closed Won**.
+   - Filter the data by **Close Date = 1/1/2025 – 30/6/2025**.
+   - Change **Query Limit** to **1,000**.
+   - Edit your columns as you see fit – see the following screenshot.
+   - Select the **Formatting** tab and choose a new table **theme**.
+   - Change the table's **spacing**. Select **Fit To Widget** under **Column Width**.
+   - Modify the **Header** details.
+   - Add a table background color.
+   - Remove the row index column.
+   - Click on the **Column** tab and add **Conditional Formatting** to a measure column – see **Figures 6.15 and 6.16**.
+   - Experiment with the other settings under the **Column** tab.
+   - **Save** your lens.

Your edited table columns should look as follows:

![](img/06379e43783b9ddfee45e9da5192efe1_171_0.png)

Figure 6.14 – Editing table columns

Your modified table might look something like this:

| AccountId.Name | OwnerId.Name | Name | AccountId.Industry | Amount ↓ | Close Date |
|---|---|---|---|---|---|
| Carr889 Inc | Johnny Green | Opportunity for Norman1354 | Communications | 5,294,610 | 2025-01-11 |
| Stevenson954 Inc | Eric Gutierrez | Opportunity for Daniel875 | Insurance | 5,023,650 | 2025-05-08 |
| Nichols662 Inc | Catherine Brown | Opportunity for Alvarado1597 | Manufacturing | 4,824,325 | 2025-04-28 |
| Flores188 Inc | Kelly Frazier | Opportunity for Cunningham175 | Retail | 4,357,300 | 2025-02-16 |
| Torres548 Inc | Evelyn Williamson | Opportunity for Watson580 | Biotechnology | 4,090,680 | 2025-05-23 |
| Watts685 Inc | John Williams | Opportunity for Rogers1242 | Biotechnology | 3,907,275 | 2025-04-01 |
| Rios830 Inc | Johnny Green | Opportunity for McDonald1503 | Apparel | 3,851,250 | 2025-02-03 |
| Doyle498 Inc | Chris Riley | Opportunity for Martin1417 | Transportation | 3,712,300 | 2025-03-31 |
| Fleming816 Inc | Laura Palmer | Opportunity for Smith309 | Insurance | 3,709,600 | 2025-02-09 |
| Wood74 Inc | Johnny Green | Opportunity for Jackson1400 | Engineering | 3,555,960 | 2025-01-13 |
| Rodriguez608 Inc | Bruce Kennedy | Opportunity for Houston1041 | Media | 3,513,380 | 2025-05-13 |
| Joseph988 Inc | Dennis Howard | Opportunity for Weber619 | Energy | 3,474,950 | 2025-06-19 |
| Dawson161 Inc | Evelyn Williamson | Opportunity for Dennis1374 | Education | 3,463,920 | 2025-05-05 |
| Byrd23 Inc | Evelyn Williamson | Opportunity for Bates1547 | Education | 3,426,450 | 2025-06-05 |
| Rodgers79 Inc | Eric Gutierrez | Opportunity for Dennis524 | Banking | 3,390,620 | 2025-02-06 |

Figure 6.17 – Sample of a customized values table

Values tables are extremely useful for preparing tables for dashboards or diving into your data one row at a time for detailed analysis. You can also add subtotals and totals using the summary button with the Σ symbol.

Now, it's time to look at the compare table tool for diving into your data.

## Compare table

Compare table is one of the most powerful declarative tools for analyzing your data. The column editor can be used to perform the following tasks:

+   - Add calculations and filters to your table.
+   - Perform arithmetic across the columns and rows.
+   - Create labels by manipulating string values.
+   - View measures side by side.
+   - Concatenate dimension values.
+   - Create simple buckets.
+   - Add image URLs.

In the following exercise, we will perform the following steps with a compare table:

+   1. Calculate a running total for the number of won deals.
+   2. Rank sales reps by the sum of won deals.
+   3. Calculate the share of total for each rep.
+   4. Calculate a deal win % for all reps, calculated by deal count (now amount).

Let's begin by creating our first compare table:

+   1. Open **My First Lens**.
+   2. Clone **My First Lens** in a new tab and save it as **My First Compare Table**.
+   3. Change to **Compare Table** mode.
+   4. Add **Count of Rows** as a second column/measure.

Now, go into column editing mode, as shown in the following screenshot, by clicking on **Edit this Column**:

![](img/06379e43783b9ddfee45e9da5192efe1_174_0.png)

Figure 6.18 – Column editing mode
