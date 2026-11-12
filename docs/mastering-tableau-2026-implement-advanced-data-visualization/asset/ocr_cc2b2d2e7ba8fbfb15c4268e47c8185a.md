![](bbox=[0.39, 0.07, 0.66, 0.42])

# Figure 12.20: Only Relevant Values

This is advantageous to the end user in that it adjusts the number of postal codes that display to reflect only those pertinent to the cities selected in the first filter. However, using relative filters will cause additional queries to be sent to the data source and thus may degrade performance.

# Table calculation filters

Using table calculations as filters does not have the same corresponding performance enhancements as dimension or measure filters. As discussed above, dimension and measure filters reduce the returned dataset, while table calculation filters do not. In the Tableau process flow, table calculations are not rendered until after the data is returned from the data source. This means that table calculations cannot be used to generate queries to limit returned data. Or, to put it another way, table calculation filters cause all data related to a given dimension or measure to be returned, after which Tableau executes the filter on the returned dataset.

To demonstrate this, perform the following steps:

+   1. Open the workbook associated with this chapter and navigate to the Late Filter worksheet
+   2. In the Data pane, select the Superstore data source

3. Create a calculated field named Cust Name Tbl Calc with the following code:

```
LOOKUP( MAX( [Customer Name] ),0 )
```

+   4. Place Customer Name on the Rows shelf
+   5. Place Cust Name Tbl Calc on the Filters shelf and constrain to show only Aaron Bergman