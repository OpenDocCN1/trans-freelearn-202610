```
</script>
    <script type="text/javascript">goog.require("core");
</script>
    </body>
</html>
```

The first HTML import script directive tells the page to load the Closure library. The second script import directive tells the page to load our compiled code. And, finally, the third script directive requires the code, meaning to load it, and since we did not create a function but just pure code, this will effectively, and finally, run the code you wrote previously.

![](bbox=[0.055, 0.295, 0.455, 0.375])

That's it, you can now go and open the index.html page in anything but Internet Explorer (IE). Just teasing, even IE will do.

# How to do it...

Now that you have the basics on your local machine, from here this is going to be a road to JavaScript freedom.

# Autocompiling your code

Instead of have to run the cljsbuild once command each time, there is actually a second mode for the cljsbuild plugin. And, yes, you have guessed it, it is called auto:

```
lein cljsbuild auto
```

With the preceding command, any change to the core.cljs file will trigger a new compilation of your code. In the ClojureScript file, let's update the Clojure code with the following code: