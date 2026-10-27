The product is removed from the cookie if the number in the Quantity field equals zero. In other cases, we will create or update the quantity value in cookies.

![](bbox=[0.28, 0.19, 0.72, 0.39])

Let's check the preceding information. Return to the Resources tab and navigate to the localhost tree item. Click on the Refresh icon at the bottom of the window to see the list of cookies associated with our server.

![](bbox=[0.17, 0.47, 0.83, 0.65])

Now, close the index.html page and open it again. Information about the selected products along with their specified quantity and number will be available here. Click on the Check Out button to invoke a cart.checkOut method to show a message about the paid items and remove the cookies from them. The code is as follows:

```
checkOut.onClick.listen((Event event) {
  cart.checkOut();
});
```