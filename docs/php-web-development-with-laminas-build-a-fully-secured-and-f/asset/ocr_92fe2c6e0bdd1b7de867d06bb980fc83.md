Whatstore Home

It looks a good purchase, DUDE!

# Close purchase order

| code | name | price | amount |
| --- | --- | --- | --- |
| 1 | ENCHANTED HAMMER | 9 | 1 |
| Total | | | 9 |

Dispatch purchase order

Keep buying

© 2022 Powered by Sheet Software

Figure 11.8: The page to close the purchase order

You can see that the message at the top has the customer’s name. This value is recovered from the session with `Customer :getCustomer`. In the next section, we will understand the code behind the page for closing a purchase order.

# Order closing

In the previous section, we saw the customer redirected to the `localhost:8000/store/order` endpoint. To conclude the purchase, the customer must click on the **Dispatch purchase order** button. This action triggers the `closeOrder` function, contained in the `index.js` file:

# index.js

```
var closeOrder = function() {
    var api = $("#api" ).attr("url") + '/0';
    var target = $("#target" ).attr("url");
    $.post(api).done(function(data) {
        if (!data.inserted)
        {
            alert('purchase order has been not inserted!!');
            return;
        }
    });
}
```