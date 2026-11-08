
## Subreports in Pentaho Report Designer

Subreports in Pentaho Reporting may be included in any band of a report, except for the page header and page footer bands. Subreports receive a DataRow of parameters, determined by the current state of their parent report when rendering the subreport. These parameters may be used when executing a subreport query or referenced directly in the subreport. Within subreports, you define named queries that may reference the DataRow of parameters passed in, allowing a subreport to query only the currently scoped data and not the entire data set available to the parent report.

Subreports may be of any length, may be included in other subreports in a nested fashion, and may also be presented alongside one another in their parent report. When spanning multiple pages, special considerations must be made, including how to handle the master report's page header, page footer, and group headers. This chapter will cover all these topics in the following examples.

## Subreport example

In this example, you will build a report which includes a very classical use case scenario: a master-detail report where the master is represented from orders (with order number and date) and detail is represented by a subreport containing products, quantity, and price. To begin with, let's start with an empty report (clicking on the blank report icon (□) of the Report Designer) and a JDBC data source created as described in Chapter 6, *Configuring JDBC and Other Data Sources*. The JDBC data source is supposed to be defined on the `SampleData (Local)` connection and the following content:

```
SELECT
"PUBLIC"."ORDERS"."ORDERNUMBER",
"PUBLIC"."ORDERS"."ORDERDATE"
FROM
"PUBLIC"."ORDERS"
```

From here, drag the `ORDERNUMBER` and `ORDERDATE` fields and drop them into the `Details` band of the empty report. Change `ORDERNUMBER` style to bold and stretch `ORDERDATE` to cover the remaining width of the page.

Till now, nothing is really different from what you learnt in the previous chapters. Let's now add a subreport representing the details of the order. To add the subreport, drag the subreport icon (≡) from the element palette and drop it below the fields, in the `Details` band. Once dropped, a modal window will open with the question: do you want to include an inline-subreport or a banded-subreport? Choose an inline-subreport if you want to cover a customizable rectangular area to put the content inside or choose the banded-subreport if you want to place the content in sub-band included in the report's band. Click on the Inline button and another tab with a subreport icon (≡) will open, similar to a brand new report.

The new tab contains the subreport and it's really a new report inside the master report. The subreport is made of data sources, layout, elements, and everything you have seen until now while reading this book. Being a real report, a window asks you to specify the data source. In our example, we will create a brand new JDBC data source, using the `SampleData (Local)` connection and the following query. To create the data source, press the well-known plus green icon (⊕) and
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

Before defining the content of the subreport, go back to the master report (click on the report tab in the report canvas) and move the subreport element immediately under the ORDERDATE field. Stretch it to cover the entire width of the ORDERDATE element, until the right edge of the page.

Back to the subreport tab, drag the three fields and drop them into the Details band of the empty subreport. First, place the QUANTITYORDERED and PRICEEACH fields on the right of the band and then place the PRODUCTCODE field on the left, stretching it to cover the entire free space.

Lastly, let's manage the ORDERNUMBER parameter used in the subreport or, more precisely, used in the subreport's data source. With this example of use of the subreport in the Details band of the report, each subreport instance depends on one order in particular. To pass the order number from the master report to the subreport, a parameter must be configured.

As you learnt in Chapter 8, *Parameterization, Functions, Variables, and Formulas*, the report parameters live in the report explorer, in particular, in the Data tab. Also, in case of subreports, the parameters can be managed from the same place, with two main differences. The first difference is that the subreport parameters are always used to move values from/to the master report and not asked to the user. The second difference is that the subreport parameters are grouped in import parameters and export parameters. As you can expect, import parameters are used to pass values from the master report to the subreport, and export parameters are used to pass values from the subreport to the master report.

In our example, you need to import a parameter to use the ORDERNUMBER values in the subreport. To define the parameter, right click on the Parameters item in the report explorer and fill the Import Parameter section, as described in the following screenshot. To add the new input parameter, click on the green plus icon (⊕) and fill Outer Name with the name of the field in the master-report. Use Inner Name to name the local parameter in the subreport (in our case, we are going to use the same name for both).

![](img/9337ac94537bf7e092e1ea6531dce8fd_501_0.png)

Now that the master-report and the subreport are defined, the Details band of your report should look like the following screenshot:

![](img/9337ac94537bf7e092e1ea6531dce8fd_501_1.png)

![](img/9337ac94537bf7e092e1ea6531dce8fd_501_2.png)

*If you are repeating the exercise of creating the report in your development environment, you can see that the available space in the subreport is reduced. Indeed, you cannot drop the element on the left side of the bands, and this space has a grey background with a white grid. This means that the width of the subreport in the master report does not cover the entire page width and only the right portion of the bands can be used. If you go into the master report and enlarge (or reduce) the width of the subreport element, you will see the subreport will be enlarged (or reduced) accordingly. Have this in mind when you use the subreports, because it will affect the available space you will be able to fill with elements.*

Click the eye icon (👁) in the report canvas and the resulting report should look like the following screenshot:

10,100    Mon Jan 06 00:00:00 CET 2003
          S24_3969    49    34
          S18_2248    50    68
          S18_1749    30    172
          S18_4409    22    87
10,101    Thu Jan 09 00:00:00 CET 2003
          S18_2795    26    145
          S24_2022    46    54
          S24_1937    45    31
          S18_2325    25    151
10,102    Fri Jan 10 00:00:00 CET 2003
          S18_1367    41    50
          S18_1342    39    123
10,103    Wed Jan 29 00:00:00 CET 2003
          S24_2300    36    102
          S18_2432    22    54
          S32_1268    31    104
          S10_4962    42    129

![](img/9337ac94537bf7e092e1ea6531dce8fd_502_0.png)

In the https://github.com/fcorti/pentaho-8-reporting-for-java-developers repository, more precisely, in the chapter 10 - Sub-reports and cross tabs/my_reports path, you can find the subreport.prpt report developing exactly the example described here. Feel free to open it with Report Designer and play with settings and parameters.

## Other subreport examples

Before leaving the subreport examples, let's list some use cases where subreports can help a lot; the intention here is to share a practical reference, not an exhaustive and final list:

- **Details in a report**: The first and an easy example of subreport is a portion of the space of the report where some details about an entity are presented. As an example, you can think about the sender of a letter or the company details in an invoice.
- **Multi-page subreports**: Quite similar to the example described earlier, you can think about a report that includes a large subreport that may span multiple pages. As an example, you can think about a catalog of products or an inventory list. In this case, the subreport will be defined in the `Report Header` band instead of the `Details` band, but everything else would work in exactly the same way as you saw here. The difference is that the data source in the subreport could retrieve a long list of data, that may span multiple pages.
- **Chart subreports**: There are no limitations to the elements you can use in subreports. For example, you can use a chart, having an appealing result.
- **Side by side subreports**: Another capability of subreports is to be able to place two or more subreports horizontally, beside one another, within a report band. Using this capability, you can specialize subreports with dedicated data sources, extracting data from different tables or sources (not necessarily one source only).
- **Multiple subreports**: There are no limitations to the number of subreports that you can use. Combining the examples shared in this section, you can create very complex reports with a modular approach. This approach can be suggested if you have to develop changes after a long period of time or in a team with different developers.

![](img/9337ac94537bf7e092e1ea6531dce8fd_503_0.png)

> _As you are learning here, subreports are very powerful, but there is a disadvantage that you should be aware of: subreports can be very expensive in terms of consuming resources. Do not abuse their use._
