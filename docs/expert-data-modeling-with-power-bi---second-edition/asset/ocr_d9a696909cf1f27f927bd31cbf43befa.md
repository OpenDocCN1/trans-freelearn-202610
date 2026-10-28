the changes, save the file as a new file, and name it Chapter 5, Common Data Preparation Steps, Fixed PostCode.pbix.

Now, click the Data view on Power BI Desktop, navigate to the Geography table, and look at the PostalCode column’s data to ensure the changes we made so far fixed that issue. The following image shows the results:

![](bbox=[0.175, 0.26, 0.825, 0.572])

Figure 5.5: Confirming that the blank postalCode issue is resolved

As the preceding image shows, the PostalCode column no longer contains blank values.

Now that we’ve solved the first issue, let us focus on the next issue, which is incorrect results when using the Date column from the Date table and the Internet Sales measure on a table visual.

To reproduce the issue, we put a table visual on the report canvas, select the Date column from the Date table and the Internet Sales measure, and the issue quickly appears. As shown in the following image, the results do not look to be correct: