# Button functions

There is also only one function we need to go over. One of the main uses of this function is that we usually pass it in when we instantiate the button object.

+   - **trigger()**: This function is called when the control button is clicked (assuming it is an `OpenLayers.Control.TYPE_BUTTON` control). If we don't define it when creating our button (or define it after the button is created), nothing will happen when we click on our button. When passing in this as a property while creating your button object, a function return code (a function's name) should be passed in and not an actual function. So, to define this on instantiation, create a function outside the `new OpenLayers.Control.Button` call and then refer to that function's name as the trigger. For example,

```
var my_func = function(){ alert( 'Stop clicking me'); }
var my_button = new OpenLayers.Control.Button({ trigger: my_func
});
```

# Creating a custom button

You know a bit more about the Button class now, so let's put that knowledge to use. We'll create a fairly simple custom button (of control type `TYPE_BUTTON`) that will update the base layer's opacity and zoom the map to a random spot. We'll cover the few basic things that should be included when creating a button:

+   - Specifying the button's `displayClass` to give it style (optional)
+   - Creating a function and setting it as the trigger function
+   - Adding the button a panel, and then adding that panel to a map

# Time for Action – creating a simple button

Let's create a simple button that has a CSS class and contains a trigger function that does a little something when we click on it.

+   1. Create a new page using the map template from Chapter 1. We'll be creating a button and a panel to place the button in (although, this time, the panel will be inside the map).