![](bbox=[0.27, 0.09, 0.68, 0.39])

Previously in this chapter we showed an example usage of observables concatenation to create a search box (or any textbox), which takes an action when the user hits the Enter button or when it clicks on a related button. The merge() operator is perfect for implementing this behavior in a web page, and this can be seen in the following example:

```
<html>
<head></head>
<body>

<input type="text" id="my_input" />
<button id="my_button">Search</button>
<span id="my_span"></span>

<script src="https://cdnjs.cloudflare.com/ajax/libs/rxjs/4.1.0/rx.all.js"></script>
<script>
var enterKeyUpObservable = Rx.Observable
.fromEvent(document.getElementById("my_input"),"keyup")
.filter(function(e){
return e.keyCode===13;
});

var searchClickObservable = Rx.Observable
.fromEvent(document.getElementById("my_button"),"click");

var enterKeyUpOrSearchClickObservable =
searchClickObservable.merge(enterKeyUpObservable);
</script>
</body>
</html>
```