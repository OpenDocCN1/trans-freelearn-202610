```
"java.util.Random@6337c201"]
```

We can also check the imports defined in each namespace with ns-imports:

```
(ns-imports 'helloagain)
;...
```

By default, Clojure does some work for us, to make sure the basic Java objects are already available in each new namespace.

In the same way as we can import Java objects, we can go along and require other Clojure namespaces in the current namespace. This is done through the :require keyword in the namespace definition:

```
helloagain=>  (ns helloagain (:require [clojure.string :refer :all]))
WARNING: reverse already refers to: #'clojure.core/reverse in namespace: helloagain, being replaced by:
#'clojure.string/reverse
WARNING: replace already refers to: #'clojure.core/replace in namespace: helloagain, being replaced by:
#'clojure.string/replace
nil
helloagain=> (reverse "helloagain")
"niagaolleh"
```

The newly referred functions can be seen using ns-refers:

```
(ns-refers 'helloagain)
; ... (somewhere clojure.string ...)
```

# Loading namespaces from files

Now, effectively, the ns calls will mostly be located at the top of each file. Let's say we want to keep the code we have now on this helloagain namespace; we will create a helloagain.clj file and copy the following content:

```
(ns helloagain
  (:import [java.util Random])
  (:require [clojure.string :refer :all]))
```