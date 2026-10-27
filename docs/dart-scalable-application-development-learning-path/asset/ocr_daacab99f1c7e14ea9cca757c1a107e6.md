```
* JS_Proxy based analog:
*/
events() {
    // We remove 'function' and add 'event' here
    $("button").click((event) {
        // Call method 'alert' of 'window'
        window.alert('You click on button');
    });
}
```

All we need to do here is just remove the function keyword, because anonymous functions on Dart do not use it, and then add the event parameter. This is because this argument is required in the Dart version of the event listener. The code calls jQuery to find all the HTML button elements to add the click event listener to each of them. So when we click on any button, a specified alert message will be displayed. On running the application, you will see the following message:

![](bbox=[0.27, 0.4, 0.73, 0.55])

# How to use effects in jQuery

The jQuery supports animation out of the box, so it sounds very tempting to use it in Dart. Let's take a look at the following code snippet:

```
/**
 * jQuery code:
 *
 * $("p").click(function() {
 *     this.hide("slow",function(){
 *         alert("The paragraph is now hidden");
 *     });
 * });
 * $(".box").click(function(){
 *     var box = this;
```