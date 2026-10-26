2. Create measures for the correlation coefficient, slope, Y-intercept, and coefficient of determination (R-squared):

```
MktSalesCorrelNum = SUMX(MktSalesRegression,
    ( (MktSalesRegression[Marketing Amount] - AVERAGE(MktSalesRegression[Marketing Amount]) ) *
      ( MktSalesRegression[Internet Sales] - AVERAGE(MktSalesRegression[Internet Sales]) )
    ) )
MktSalesCorrelDenomX = SUMX(MktSalesRegression,
    ( MktSalesRegression[Marketing Amount] - AVERAGE(MktSalesRegression[Marketing Amount]) )^2 )
MktSalesCorrelDenomY = SUMX(MktSalesRegression,
    ( MktSalesRegression[Internet Sales] - AVERAGE(MktSalesRegression[Internet Sales]) )^2 )
Mkt-Sales Correl = DIVIDE([MktSalesCorrelNum], SQRT([MktSalesCorrelDenomX]*[MktSalesCorrelDenomY]) )
Mkt-Sales R Squared = [Mkt-Sales Correl]^2
MktSalesSlope = DIVIDE([MktSalesCorrelNum],[MktSalesCorrelDenomX])
MktSales Intercept = AVERAGE(MktSalesRegression[Internet Sales]) - ( [MktSalesSlope]*AVERAGE(MktSalesRegression[Marketing Amount]) )
```

3. Click New table from the ribbon of the Modeling tab and use the following code:

```
Residuals =
    VAR Intercept = [MktSales Intercept]
    VAR Slope = [MktSalesSlope]
RETURN
    ADDCOLUMNS(MktSalesRegression,"Y Intercept",Intercept,"Slope",Slope,
    "Predicted Internet Sales", ([Marketing Amount]*Slope) + Intercept,
    "Residual",[Internet Sales] - (([Marketing Amount]*Slope) + Intercept))
```

4. Create the following measures:

```
Residuals Amount = SUM(Residuals[Residual])
Residuals Average = CALCULATE(AVERAGE(Residuals[Residual]),ALL(Residuals))
Residuals Sample Std Dev = CALCULATE(STDEV.S(Residuals[Residual]),ALL(Residuals))
Residuals Z Score = DIVIDE([Residuals Amount] - [Residuals Average],[Residuals Sample Std Dev])
Regression Line Message = "Regression Line: Y= " & FORMAT([MktSalesSlope],"#,###.##") & "X" & "+" & FORMAT([MktSales Intercept],"#,###")
```