```
Last Month Internet Sales =
    VAR __LastMonth = MAX('Residuals'[YearMonthSort])
    RETURN
        LOOKUPVALUE('Residuals'[Internet Sales],[YearMonthSort],__LastMonth)
Last Month Predicted Internet Sales =
    VAR __LastMonth = MAX('Residuals'[YearMonthSort])
    RETURN
        LOOKUPVALUE('Residuals'[Predicted Internet Sales],[YearMonthSort],__LastMonth)
```

+   5. From the Report view, create card visuals to display the Last Month Internet Sales, Last Month Predicted Internet Sales, Mkt-Sales Correl, Mkt-Sales R Squared, and Regression Line Message measures.
+   6. Create a scatter chart visualization using the Marketing Amount column (sum) from the MktSalesRegression table as the X-axis, the Internet Sales column (sum) from the MktSalesRegression table as the Y-axis, and the YearMonth column from the MktSalesRegression table as Value.
+   7. Add a Trend line from the Analytics pane.
+   8. Create a second scatter chart visualization using the Predicted Internet Sales column (sum) from the Residuals table as the X-axis, the Residuals Z Score column (sum) from the Residuals table as the Y-axis, and the YearMonth column from the Residuals table as Values.

![](bbox=[0.18, 0.52, 0.92, 0.86])

Figure 9.22: Regression report page

With this report design, the user can instantly perceive the strength of the relationship via the Marketing Amount and Internet Sales by YearMonth scatter chart and the high values for the correlation and R-squared cards, as shown in Figure 9.22. The Sum of Predicted Internet Sales and Residuals Z Score by YearMonth scatter chart helps to identify the months with relatively large variations.