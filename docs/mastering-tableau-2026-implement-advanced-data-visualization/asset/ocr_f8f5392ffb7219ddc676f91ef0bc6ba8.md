10. In the resulting dialog box, select **Specific Dimensions** and then observe the results for each of the following selections:

| Select Specific Dimension | Data densification activated/deactivated |
|---|---|
| **Location Description** | Activated |
| **Year** | Activated |
| **Location Description and Year** | Deactivated |
| **No selection** | Deactivated |

# Table 6.1: Dialog box

The preceding exercise illustrates the following rule for deploying domain completion:

> *Given a crosstab with discrete dimensions on the Rows and Columns shelves, utilizing a table calculation (in this case, the Index field) in which at least one dimension (but not all dimensions) is addressed activates domain completion.*

A key term in the preceding rule may have been confusing: addressed. Partitioning and addressing were covered in Chapter 5, Introducing Table Calculations, but will be defined again here to ensure understanding of this rule. Consider the following information from the Tableau documentation (https://help.tableau.com/current/pro/desktop/en-us/calculations_tablecalculations.htm):

> *The dimensions that define how to group the calculation, that is, that define the scope of the data it is performed on, are called partitioning fields. The table calculation is performed separately within each partition. The remaining dimensions, upon which the table calculation is performed, are called addressing fields, and determine the direction of the calculation.*

When editing a table calculation, you can choose to select/deselect specific dimensions. When a dimension is selected, that dimension is used to address the table calculation. When a dimension is not selected, the dimension is used to partition the table calculation. The following screenshot of a Table Calculation editing dialog box demonstrates addressing on Location Description and partitioning on Year.