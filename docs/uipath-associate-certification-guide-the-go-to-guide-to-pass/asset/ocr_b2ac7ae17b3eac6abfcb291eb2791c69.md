10. Below the Multiple Assign activity, use the For Each Row activity and pass dt_invoiceParticulars into the **DataTable** property field.

11. Inside the body of the For Each Row activity, drag and drop the Type Into activity and indicate the **Quantity** text field on the Order Entry CRM web page. Type in row(0).ToString.Trim under the **Text** property.

Let's look at the selector for this Type Into activity:

![](bbox=[0.17, 0.245, 0.79, 0.645])

Figure 15.26 - The selector for the Type Into activity of the Quantity text field

If you look at the selector, it has a `tableRow='2'` attribute, which means it will type in the second row of the table for every iteration. However, you need to type in the new row for every new line item; hence, replace 2 with the `tableRow` variable.

The new selector should look as follows:

```
<webctrl parentid='dataTable' tag='INPUT' colName='Quantity' tableCol='1' tableRow='{{tableRow}}' />
```

> **Note**
> If the tableCol and tableRow attributes are not present in the selector, use the **UI Explorer** window and add them to the selector.