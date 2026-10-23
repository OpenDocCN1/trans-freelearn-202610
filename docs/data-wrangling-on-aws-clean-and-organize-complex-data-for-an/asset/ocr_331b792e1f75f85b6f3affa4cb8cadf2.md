![](bbox=[0.15, 0.107, 0.85, 0.311])

Figure 9.67: Exploring the ticket_purchase_hist table

The table has details on the ticket ID, transaction date, purchase price, and purchase person ID. We can use this table alone to visualize the number of tickets sold per day.

We will use a Matplotlib scatterplot to visualize the tickets sold on a daily basis. Please note this is only test data and we ran the stored procedure to populate ticket purchase history earlier in this chapter. In the sample data shown in the screenshot, the stored procedure was executed on two days to generate the ticket purchase data volume:

```
import matplotlib.pyplot as plt
plt.xticks(rotation=-90)
plt.scatter(ticket_purchase_hist['transaction_date_time'].dt.date, ticket_purchase_hist['purchase_price'])
```

![](bbox=[0.15, 0.549, 0.45, 0.789])

Figure 9.68: Matplotlib visualization of ticket purchase history data on a daily basis

We have used an xticks rotation argument to make the date values easy to read. We then create a scatterplot with transaction dates and purchase prices as shown here. We can see that there are more tickets sold in the lower price ranges (dots populated densely), but there are also some tickets sold at a high price range. Let us explore and find out how many tickets are sold above the price range of