This example is simple and it works the same way the flatMap() operator works; it will give you the following output:

2
3
4

We can implement the search as you type feature to see the difference; for this let's create an HTML page with the implementation and compare the result:

```
```html
<html>
    <head></head>
    <body>
        <input type="text" id="my_input"/>
        <button id="my_button">Search</button>
        <span id="my_span"></span>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/rxjs/4.1.0/rx.all.js"></script>
        <script>
            var enterKeyUpObservable = Rx.Observable
                .fromEvent(document.getElementById("my_input"),"keyup")
                .flatMap(function(e){
                    var value = document.getElementById("my_input").value;
                    return Rx.Observable
                        .just("results for: "+value)
                        .delay(Math.max((10 - value.length)*1000,100));
                });
            enterKeyUpObservable.subscribe(function(text){
                document.getElementById('my_span').innerHTML = text;
            });
        </script>
    </body>
</html>
```
```

This HTML page will expose a search box, and as you type in it it will do a fake search, showing the value used in the search as text; we are using the flatMap() operator to emulate the search feature, and we use a delay to emulate the time taken to execute the search, if you run this HTML page and type something in the search box you will see the following screenshot:

![](bbox=[0.34, 0.84, 0.63, 0.89])