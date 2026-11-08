follow the same path as described in Chapter 6, *Configuring JDBC and Other Data Sources.*

```
SELECT
"PUBLIC"."ORDERDETAILS"."PRODUCTCODE",
"PUBLIC"."ORDERDETAILS"."QUANTITYORDERED",
"PUBLIC"."ORDERDETAILS"."PRICEEACH"
FROM
"PUBLIC"."ORDERDETAILS"
WHERE
"PUBLIC"."ORDERDETAILS"."ORDERNUMBER" = ${ORDERNUMBER}
ORDER BY
"PUBLIC"."ORDERDETAILS"."ORDERLINENUMBER" ASC
```

Before defining the content of the subreport, go back the master report (click on the report tab in the report canvas) and move the subreport element immediately under the ORDERDATE field. Stretch it to cover the entire width of the ORDERDATE element, until the right edge of the page.

Back to the subreport tab, drag the three fields and drop them into the Details band of the empty subreport. First, place the QUANTITYORDERED and PRICEEACH fields on the right of the band and then place the PRODUCTCODE field on the left, stretching it to cover the entire free space.

Lastly, let's manage the ORDERNUMBER parameter used in the subreport or, more precisely, used in the subreport's data source. With this example of use of the subreport in the Details band of the report, each subreport instance depends by one order in particular. To pass the order number from the master report to the subreport, a parameter must be configured.

As you learnt in Chapter 8, *Parameterization, Functions, Variables, and Formulas*, the report parameters live in the report explorer, in particular, in the Data tab. Also, in case of subreports, the parameters can be managed from the same place, with two main differences. The first difference is that the subreport parameters are always used to move values from/to the master report and not asked to the user. The second difference is that the subreport parameters are grouped in import parameters and export parameters. As you can expect, import parameters are used to pass values from the master report to the subreport, and export parameters are used to pass values from the subreport to the master report.

In our example, you need an import a parameter to use the ORDERNUMBER values in the subreport. To define the parameter, right click on the Parameters item in report