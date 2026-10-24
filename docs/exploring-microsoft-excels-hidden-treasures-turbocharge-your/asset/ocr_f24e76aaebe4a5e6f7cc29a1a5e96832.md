# Nuance

Field Names must be enclosed in square brackets when referenced in custom or **Conditional**
**Columns in Power Query**.

VII. Click the **ABC123** button below **Else** and choose **Select a column**.
VIII. Choose **Initial Amount** from the **Output** list, as shown here:

![](bbox=[0.1338, 0.251, 0.8506, 0.491])

Figure 12.22 – The Add **Conditional Column** dialog box

IX. Click **OK** to close the **Add Conditional Column** dialog box.

# Tip

Depending on the data that you’re working with, there could be some risk in this approach
with **Conditional Column**. For this dataset, the _real_ numbers will never be anywhere near
1,000,000, so dividing by 100 does not cause an issue. This is by no means an end-all solution,
but rather an illustration of just one way to solve the problem when **Power Query** misconstrues
some of your data.

20. The **Amount** column in row **41** shows **[Initial Amount]/100** rather than the calculation result.
To resolve this, remove the double quotes around **[Initial Amount]/100** in **Power Query’s**
**Formula Bar**, as shown in **Figure 12.23**. Click the down arrow in the top right-hand corner if
you need to expand the **Formula Bar**: