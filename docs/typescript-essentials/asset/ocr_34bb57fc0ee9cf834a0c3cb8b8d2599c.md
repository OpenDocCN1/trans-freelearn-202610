![](bbox=[0.061, 0.041, 0.803, 0.382])

As you can see, we include the jQuery and Knockout libraries in head. Then, in the body of the HTML, we have the two elements we created during the jQuery example. However, each of these elements now contains bindings. The `colorPanel` div is bound to the `IsVisible` observable using Knockout’s visible binding. When the IsVisible observable changes, Knockout will evaluate the new value and manipulate the DOM to show or hide the div. The button is bound to multiple elements on the view model, the `toggleText` object, which will determine what the value of the button is, and the `toggleClick` method, which will execute anytime the click event fires.

# Tip

Knockout supports many different kinds of bindings and also has a custom binding model that will allow you to define your own bindings. Visit [www.knockoutjs.com](http://www.knockoutjs.com) to learn more.

OK, we have most of the pieces together now to dynamically bind the DOM to our TypeScript/JavaScript but we need to actually connect the view and the view model. The following code segment shows how to bind the two together:

```
```javascript
$(document).ready(() => {
    ko.applyBindings(new ViewModel());
});
```
```

As you can see, we wait for the DOM to be ready before we apply the binding. If we attempt to bind before all of the elements are available an error will occur at runtime. Once the DOM is ready, we create a new instance of our `ViewModel` class and pass it to Knockout, which will handle the rest. Optionally, you can pass in a specific element to bind the view model to. Now when we run the application it starts with the large blue square visible and the text of the button is **Hide**. However, instead of this being the end of