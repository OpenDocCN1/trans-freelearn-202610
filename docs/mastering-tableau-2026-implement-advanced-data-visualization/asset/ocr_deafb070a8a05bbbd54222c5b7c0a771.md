+   9. Select the Records data source and create a calculated field called Date with the following code:

```
IIF([Records]=1,[Start Date],[End Date])
```

+   10. Place the Date field on the Rows shelf.

+   11. Right-click on the Date field on the Rows shelf and select Show Missing Values. Note that all the dates between the start and end date settings now display:

![](bbox=[0.268, 0.217, 0.835, 0.546])

Figure 4.35: Show Missing Values

+   12. Create a parameter named Select Country A with the settings shown in the following screenshot. Note that the list of countries was added with the Add values from button: