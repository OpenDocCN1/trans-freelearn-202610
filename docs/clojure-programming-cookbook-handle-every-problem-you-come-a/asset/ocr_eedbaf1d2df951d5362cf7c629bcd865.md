Locally, we will actually use the install command to make the library available to other projects:

```
lein install
; Installed jar and pom into local repo.
```

In project.clj, you can now add a dependency on the newly created library, the usual Clojure way:

```
[cljs-two "0.1.0-SNAPSHOT"]
```

And, in a new ClojureScript file, you can reference the previous library ClojureScript code, just as if it was here:

```
(ns core2
  (:require [core]))
```

```
(def $ js/jQuery)
```

```
(.click
  ($ "body")
  core/click-slide-fn)
```

And it still clicks!

# Using a third-party library

This small section will be a simple follow-up to using third-party ClojureScript libraries. Here we want to try the ClojureScript money library, a library that helps you play with Canvas. Your project.clj will contain an added reference to a release number:

```
[rm-hull/monet "0.2.2"]
```

Now you can make use of the money namespaces, just as we have seen previously:

```
(ns core
  (:require [monet.canvas :as canvas]))
(def canvas-dom
  (.getElementById js/document "canvas"))
(def monet-canvas
```