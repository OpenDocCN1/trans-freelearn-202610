Now, when you make a request to `localhost:8000` in your browser, the resulting page will look like *Figure 11.4*:

![](bbox=[0.13, 0.15, 0.86, 0.6])

Figure 11.4: The customer home page with available products

As you can see, every product on the customer home page has a **Buy** button. When a customer clicks on this button, the product must be added to the product basket. This action is controlled by the *index.js* file, located in the *view/store/index* folder:

# index.js

```
var addToBasket = function() {
    var api = $("#api").attr("url") + '/' + $(this).
        attr('id');
    var formData = $("#product" + $(this).attr('id')
        ).serialize();
```