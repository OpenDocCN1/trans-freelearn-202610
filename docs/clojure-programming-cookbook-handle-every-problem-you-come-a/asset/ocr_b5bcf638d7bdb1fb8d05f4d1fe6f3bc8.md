```
(make-triangle 5)
;;=>       *
;;=>     ***
;;=>   *****
;;=>  *******
;;=> *********
;;=> nil
(make-triangle 6 :char "x"))
;;=>       x
;;=>     xxx
;;=>   xxxxx
;;=>  xxxxxxx
;;=> xxxxxxxxx
;;=> xxxxxxxxxxx
```

# How it works...

We have already reviewed how to define functions and how to use them. You should understand how they work after reviewing the previous section.

To define functions using `defn` is the same as vars bind to functions by `fn` as follows:

```
(defn pow-py-defn [x] (* x x))
;;=> #'living-clojure/pow-py-defn
(def pow-by-def (fn [x] (* x x)))
;;=> #'living-clojure/pow-by-def
(pow-py-defn 10)
;;=> 100
(pow-by-def 10)
;;=> 100
```

# There's more...

`clojure.repl` has some useful functions to use with other functions. To get a symbol in the specific namespace, use `clojure.repl/dir`:

```
(require 'clojure.string)
;;=> nil
(clojure.repl/dir clojure.string)
;;=> blank?
;;=> capitalize
```