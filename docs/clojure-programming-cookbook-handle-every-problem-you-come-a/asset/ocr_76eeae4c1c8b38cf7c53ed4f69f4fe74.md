simply try to grab mouse clicks and print some messages. So, this would be the following jQuery part:

```
$(document.body).click(function () {
  ..
})
```

The print click function we define as follows, in standard Clojure code:

```
(defn click-fn[]
  (println "Clicked!"))
```

Now that you have the handler, let's react on mouse clicks. The DOM element we are targeting is body. We access the JavaScript exposed jQuery object from ClojureScript via js/jQuery and we call it with the parameter "body":

```
(js/jQuery "body")
```

We have the body element as a jQuery object, so let's use the click function. In usual Clojure interop, the .click dispatching can be used:

```
(.click
  (js/jQuery "body")
  click-fn)
```

Suppose you have some text in the index.html page, say:

```
<body>
Click me!
...
</body>
```

That's right, now is the time to show some mouse-clicking power !! And yes, the message shows up, as shown in the following screenshot:

![](bbox=[0.06, 0.8, 0.4, 0.9])