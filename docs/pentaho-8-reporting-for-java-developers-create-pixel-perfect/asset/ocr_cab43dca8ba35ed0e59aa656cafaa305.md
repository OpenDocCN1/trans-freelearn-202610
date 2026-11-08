# Opening a dynamic URL from a Pentaho report

Another relevant example is an improvement of the static URL you have previously seen. In this section you are going to see two different interactions, opening dynamic URLs by clicking on a report's element. Starting from the previous report, you can click on the QUANTITY field or the BUYPRICE field (of each row). The result is a modal window containing the total cost of the product, calculated by multiplying QUANTITY * BUYPRICE. In the following screenshot, you can see what the user experience looks like:

![](bbox=[0.115, 0.38, 0.68, 0.73])

The development of this interaction is very similar to the previous one, thanks to the on-click property. The only difference with the static URL is that the value of the property is dynamically calculated using the following formula:

```
=CONCATENATE(
"alert('Total cost (Quantity * BuyPrice) = ";
[QUANTITY]*[BUYPRICE];
"!');"
)
```