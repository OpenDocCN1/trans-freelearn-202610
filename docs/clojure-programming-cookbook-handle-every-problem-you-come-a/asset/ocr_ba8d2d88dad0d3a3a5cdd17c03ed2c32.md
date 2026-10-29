Now, we saw just a few moments ago that we can force :reload on a namespace, but things get complicated when namespaces depend on each other, and you are not sure which one was reloaded and which one was not. This is where tools.namespace makes it easier for you to track all this for you. Let us start a REPL and try this out:

```
lein try org.clojure/tools.namespace "0.2.11"
```

As per the doc, the refresh function will scan all the directories on the classpath for Clojure source files, read their ns declarations, build a graph of their dependencies, and load them in dependency order. (You can change the directories it scans with set-refresh-dirs.)

So, at the REPL, the function we mostly need from tools.namespace is refresh:

```
(require '[clojure.tools.namespace.repl :refer [refresh]])
```

Now, supposing our file helloagain.clj is in the src folder so, src/helloagain.clj, and lein can find and load the file with require:

```
(require '[helloagain :refer :all])
user=> (only-one)
only one
```

We will quickly add a new function to our file:

```
(defn only-two []
  (println "only two"))
```

And call refresh to make sure we have the latest code:

```
user=> (refresh)
; :reloading (helloagain)
user> (helloagain/only-two)
; only two
```

And that is all. We have pulled the latest from our namespace code, and the required namespaces will be reloaded as needed.