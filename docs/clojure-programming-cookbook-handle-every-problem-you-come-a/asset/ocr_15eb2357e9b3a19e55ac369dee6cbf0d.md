```
(defproject chapter01 "0.1.0"
  :dependencies [
    [org.clojure/clojure "1.8.0-beta1"]
    [clj-tuple "0.2.2"]
    [mvxcvi/puget "0.9.1"]])
```

The first time we launch a new REPL we will notice all the dependencies being downloaded:

```
NicolassMacBook:chapter01 niko$ lein repl
Retrieving mvxcvi/puget/0.9.1/puget-0.9.1.pom from clojars
Retrieving fipp/fipp/0.6.2/fipp-0.6.2.pom from clojars
Retrieving org/clojure/core.rrb-vector/0.0.11/core.rrb-vector-0.0.11.pom from central
Retrieving mvxcvi/arrangement/1.0.0/arrangement-1.0.0.pom from clojars
Retrieving org/clojure/core.rrb-vector/0.0.11/core.rrb-vector-0.0.11.jar from central
Retrieving fipp/fipp/0.6.2/fipp-0.6.2.jar from clojars
Retrieving mvxcvi/arrangement/1.0.0/arrangement-1.0.0.jar from clojars
Retrieving mvxcvi/puget/0.9.1/puget-0.9.1.jar from clojars
```

This only applies the first time. The second time it occurs without extra messages, and you can now check for yourself that the library is there, and you get the expected colorized output, but not in this book:

```
user=> (require '[puget.printer :as puget])
nil
user=> (puget/pprint #{'x :a :z 3 1.0})
#{1.0 3 :a :z x}
nil
user=> (puget/cprint #{'x :a :z 3 1.0})
#{1.0 3 :a :z x}
```

# one-off

Sometimes it is great to just try things out, and have a one-off REPL to try out the new dependencies. This is where we can use a Leiningen plugin named try.

Plugins for Leiningen can be installed globally on your machine with a file named profiles.clj located here: