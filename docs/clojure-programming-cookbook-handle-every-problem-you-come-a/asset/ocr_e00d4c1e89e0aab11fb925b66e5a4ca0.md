```
(fn [x]
  (let [p (.getPath x)]
    [p (count-lines p)]
    )) (get-source-files p :ext ext))
)
;;=> #'chapter04.files/get-source-lines
```

Let's test the get-source-lines:

```
(get-source-lines ".")
;;=> (["./project.clj" 18] ["./src/chapter04/kafka.clj" 235]
;;=> ...
;;=> ["./src/chapter04/camel.clj" 103]
    ;;=> ["./src/chapter04/rabbitmq.clj" 296])
```

Let's apply the preceding code for the Clojure source code. So, we will get the Clojure source code on GitHub using git clone first:

```
$ git clone https://github.com/clojure/clojure.git
```

Then, we will pick up the file names of Clojure source files and count the number of lines of these files:

```
(get-source-lines "./clojure")
;;=> (["./clojure/test/clojure/test_clojure/pprint.clj" 20]
;;=> ["./clojure/test/clojure/test_clojure/multimethods.clj" 234]
;;=> ["./clojure/test/clojure/test_clojure/macros.clj" 113]
;;=> ["./clojure/test/clojure/test_clojure/data_structures.clj" 1303]
;;=> ....
;;=> ["./clojure/test/clojure/test_clojure/protocols.clj" 676]
;;=> ["./clojure/test/clojure/test_clojure/fn.clj" 55]
;;=> ["./clojure/test/clojure/test_clojure/other_functions.clj" 355]
;;=> ....
```

The following code counts the total number of lines of Clojure source code:

```
(reduce + (map second (get-source-lines "./clojure")))
;;=> 36584
```