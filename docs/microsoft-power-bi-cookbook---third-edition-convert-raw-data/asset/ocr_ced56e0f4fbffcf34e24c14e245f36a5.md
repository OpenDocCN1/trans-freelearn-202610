Name

Beat Forecast By

Data type

Decimal number

Minimum

0

Maximum

4

Increment

.1

Default



Figure 7.12: What-if parameter

3. Ensure the Add slicer to this page option is checked and then click the Create button.

4. Modify the Total Sales Amount measure as follows:

```
Total Sales Amount =
    VAR __BeatForecastValue = [Beat Forecast By Value]
    VAR __ForecastIncrease =
        IF(ISBLANK(__BeatForecastValue),1,1+__BeatForecastValue)
    RETURN
        SWITCH(TRUE(),
            ISBLANK(COUNTROWS('Sales Forecast')),
                IF(
                    MAX('Dates'[FullDateAlternateKey])<[Today],
                    SUM('Internet Sales'[SalesAmount]),
                    BLANK()
                ),
            SUM('Sales Forecast'[Forecast Sales]) * __ForecastIncrease
        )
```

Users can now use the Beat Forecast By slicer to control what-if scenarios, regarding exceeding the sales forecast by the specified percentages.

# See also

+   - Create and use parameters to visualize variables in Power BI Desktop: https://bit.ly/4b2bf3H