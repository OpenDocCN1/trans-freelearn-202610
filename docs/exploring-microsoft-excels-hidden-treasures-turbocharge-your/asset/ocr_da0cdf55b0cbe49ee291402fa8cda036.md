# 2.  Choose a scenario, such as 345 Pine Street – 30 Year Loan, and click on Show.

# 3.  As shown in Figure 6.5, cells C2:C5 should reflect the inputs for your scenario:

![](bbox=[0.132, 0.167, 0.853, 0.307])

# Figure 6.5 – The scenario applied to the worksheet

Clearly, being able to apply various scenarios to a worksheet can help improve spreadsheet integrity because you can reuse existing calculations in the worksheet. Also, Scenario Manager offers a means to back up sets of assumptions, particularly in situations where certain users might have a predisposition toward overwriting assumptions in a worksheet. However, one of the best aspects of Scenario Manager is the ability to run a scenario report that allows you to compare the inputs and outputs of your scenarios.

# Scenario reports

Excel offers two different types of scenario reports:

+   • Scenario Summary: This provides comprehensive detail, including listing all the inputs used in the scenario and up to 32 result cells that are formulas that reference the inputs.

+   • Scenario PivotTable: This compares the results of the summaries in a PivotTable format but does not display the input values used in the scenario.

Each report appears on a new worksheet in your workbook every time you generate the report. Neither report contains any live formulas, so you’ll need to recreate the scenario summaries any time you change the values in an existing scenario or add/delete scenarios.

# The Scenario Summary report

The Scenario Summary report gives you the best comparison of your scenarios. The report shows the cell addresses for both inputs and outputs. In this context, an output is a formula that relies on one or more of the input cells. To create the Scenario Summary report, perform the following steps:

+   1. Choose Data | What-If Analysis | Scenario Manager | Summary.