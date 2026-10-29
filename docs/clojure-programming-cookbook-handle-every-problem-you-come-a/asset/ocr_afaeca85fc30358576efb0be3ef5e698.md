```
:main cljc-example.quil-demo
:optimizations :advanced
:pretty-print false}]]}
```

The min option optimizes the highest optimization for the performance and generates the smallest JavaScript code into a single file. It changes names of variables and functions into shorter names. The min option generates obfuscated code. On the other hand, the dev option does no optimization and is used for developing and debugging; however, it gives slower performance. resources/public/js/compiled/cljc_example.js is the single JavaScript code generated from ClojureScript.

The auto option automatically generates JavaScript code when ClojureScript code is modified. The following option is useful for debugging:

```
$ lein cljsbuild auto min
```

Now, we will run the demo in your browser. The following command opens a new browser session and launches the demo:

```
$ browser resources/public/index.html
```

Only the following files are necessary to run the ClojureScript demo:

![](bbox=[0.055, 0.6, 0.69, 0.795])

Follow these simple steps to run the demo on your github.io:

+   1. Create your GitHub account if you don't have it.