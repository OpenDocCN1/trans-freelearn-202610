![](bbox=[0.14, 0.09, 0.21, 0.13])

path, you can find the *crosstab_mdx_datasource.prpt* report developing exactly the example described here. If you use this report, be aware that you will have to update the data source to point to the right path containing the *steelwheel.mondrian.xml* file.

As another example, let's develop a similar report using an SQL query instead of an MDX query. To begin with this second example, create an empty report (click on the blank report icon (□) of the Report Designer) and setup a JDBC data source, created as described in Chapter 6, *Configuring JDBC and Other Data Sources*. The JDBC data source is supposed to be defined on the `SampleData (Local)` connection with the following query:

```
SELECT
"PUBLIC"."QUADRANT_ACTUALS"."REGION",
"PUBLIC"."QUADRANT_ACTUALS"."DEPARTMENT",
SUM("PUBLIC"."QUADRANT_ACTUALS"."ACTUAL") AS "ACTUAL",
SUM("PUBLIC"."QUADRANT_ACTUALS"."BUDGET") AS "BUDGET",
SUM("PUBLIC"."QUADRANT_ACTUALS"."VARIANCE") AS "VARIANCE"
FROM
"PUBLIC"."QUADRANT_ACTUALS"
GROUP BY
"PUBLIC"."QUADRANT_ACTUALS"."REGION",
"PUBLIC"."QUADRANT_ACTUALS"."DEPARTMENT"
```

Once done, point on the `Master Report` element in the Structure tab of report explorer. In particular, right click on the `Master Report` element and select the Add Crosstab item. After clicking the item, a cross tab wizard will be shown, as in the following screenshot. Fill in the fields, paying attention to change the Aggregation function to `Sum` for `ACTUAL`, `BUDGET`, and `VARIANCE`: