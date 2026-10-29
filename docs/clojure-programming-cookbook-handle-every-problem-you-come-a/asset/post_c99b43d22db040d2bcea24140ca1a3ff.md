
```
; ;=> "Elapsed time: 140.624018 msecs"
; ;=> nil
(s/set-fn-validation! false)
; ;=> nil
(time
  (dotimes [n 100000]
    (int-add 1 2)))
; ;=> "Elapsed time: 28.948555 msecs"
; ;=> nil
```

In terms of performance, using set-fn-validation is faster than with-fn-validation/without-fn-validation to switch validations. Lastly, we will test the performance of native + function:

```
(time
  (dotimes [n 100000]
    (+ 1 2)))
; ;=> "Elapsed time: 7.058001 msecs"
; ;=> nil
```

Additions for two numbers are too small to evaluate performance of prismatic/schema. Let's measure performance by calculating additions of a hundred numbers:

```
(time
  (dotimes [n 1000000]
    (apply int-add (range 100))))
"Elapsed time: 3387.52923 msecs"
; ;=> nil
```

```
(time
  (dotimes [n 1000000]
    (apply + (range 100))))
"Elapsed time: 3681.930561 msecs"
; ;=> nil
```

There seems to be no difference between using schema and not using schema.

## Generating test data

prismatic/schema can be used to generate test data. prismatic/schema-generators generates test data from schemas. prismatic/schema-generators is still an alpha version, but very useful to generate test data and understand the data structure. So, let's see how it works.

First, we will add schema-generators.generators to the :require section in the namespace declaration:

```
(ns schema-example.clj-example
  (:require [schema.core :as s]
            [schema.coerce :as coerce :include-macros true]
            [schema-generators.complete :as c :include-macros true]
            [schema-generators.generators :as g :include-macros true]
            [schema-generators.complete :as gc]
  ))
;;=> nil
```

Let's add another map key and its value is an enum type:

```
(def Book
  {
    :name s/Str
    :author [s/Str]
    :keyword #{s/Keyword}
    :publisher s/Str
    :category (s/enum :it :romance :business-money)})
;;=> #'schema-example.clj-example/Book
```

Then, we will create test data from the Book:

```
(g/generate Book)
;;=> {:name "gWTFZ>'", :author ["ECg5Q" " i>Y8z\\2"], :keyword #{}, :publisher "s@", :category :it}
```

The following code will generate 100 data:

```
(g/sample 100 Book)
;;=> ({:name "", :author [], :keyword #{}, :publisher "", :category :business-money}
;;=> {:name "", :author [""], :keyword #{:Wt}, :publisher ">", :category :it}
;;=> {:name "z", :author [], :keyword #{:j}, :publisher "jK", :category :business-money}
;;=> ......
```

It's easy to generate random data:

```
(g/sample 10 s/Str)
;;=> ("" "'" "##" "r8" "" "'Uqn3" "" "F8602B1" "AR~2," "")
(g/sample 10 s/Int)
;;=> (-1 -1 -2 1N 0N -9 -1N 23 2 -6)
(g/sample 10 s/Num)
;;=> (0 0.75 0 1 -1 2.0 -4 -2 2.0 0.5)
```

schema-generators.complete creates complete data for defined schemas:

```
(gc/complete {:name "foo" :keyword #{:bar}} Book)
;;=> {:name "foo", :author ["ft?" "yY31"], :keyword #{:bar},
:publisher "kw", :category :romance}
```

## There's more...

prismatic/schema runs on ClojureScript. We will run Figwheel to test it.

## Data coercion with prismatic/schema

Before we learn about this section, let's run Figwheel REPL:

```
$ lein figwheel
```

Then, define schema-example.cljs-example namespace and declare schema libraries as follows:

```
(ns schema-example.cljs-example
  (:require [schema.core :as s :include-macros true]
            [schema.coerce :as coerce :include-macros true]
            [schema-generators.complete :as c :include-macros true]
            [schema-generators.generators :as g :include-macros true]
            [schema-generators.complete :as gc :include-macros true]))
;;=> nil
```

Using ClojureScript, the :include-macros option is necessary. Let's assume an order has two products. defschema is a macro for defining

schemas and recording the name of the schema in the metadata:

```
(s/defschema RequestOrder
  { :id s/Str
    :product s/Str
    :qty s/Int
    :unit-price s/Num
    :product2 s/Str
    :qty2 s/Int
    :unit-price2 s/Num})
;;=> #'schema-example.cljs-example/RequestOrder
```

We will define ReplyOrder, whose format is slightly different from RequestOrder:

```
(s/defschema ReplyOrder
  { :order-id s/Str
    :ordered-product [[(s/one s/Str "s") (s/one s/Int "s") (s/one s/Num "s")]]
    })
;;=> #'schema-example.cljs-example/ReplyOrder
```

Two orders are grouped into :order-product and the data type of value is arrays of array. prismatic/schema/one restricts the orders of the sequences of arrays. The previous schema restricts each product is [String, Integer,Number].

We will write a mapping between RequestOrder and ReplyOrder as follows:

```
(defn RequestOrder->ReplyOrder
  [{:keys [id product qty unit-price product2 qty2 unit-price2]
    :as request-params}]
  {:order-id id :ordered-product [[product qty unit-price]
                                   [product2 qty2 unit-price2]]})
;;=> #'schema-example.cljs-example/RequestOrder->ReplyOrder
```

Then, let's define order-coercer and test it:

```
(def order-coercer
  (coerce/coercer ReplyOrder
                   {ReplyOrder RequestOrder->ReplyOrder}))
;;=> #'schema-example.cljs-example/order-coercer
(order-coercer
```

```
{:id "10" :product "pencil" :qty 5 :unit-price 1.0 :product2 "note" :qty2 5 :unit-price2 3.0})
    ; ;=> {:order-id "10", :ordered-product [["pencil" 5 1] ["note" 5 3]]}
```

We can see that an input map is transformed into an array of arrays. Next, we will try another coercer. The following coercer transforms products into an array of maps:

```
(s/defschema ReplyOrder2
  { :order-id s/Str
    :ordered-product
    [{(s/required-key :product-name) s/Str
      (s/required-key :qty) s/Int
      (s/required-key :unit-price) s/Num}]})
;;=> #'schema-example.cljs-example/RequestOrder
(defn RequestOrder->ReplyOrder2
  [{:keys [id product qty unit-price product2 qty2 unit-price2]
    :as request-params}]
  {:order-id id
   :ordered-product [
                    {:product-name product :qty qty :unit-price unit-price}
                    {:product-name product2 :qty qty2 :unit-price unit-price2}]})
;;=> #'schema-example.cljs-example/RequestOrder->ReplyOrder2
```

Then, let's test it:

```
(def order-coercer2
  (coerce/coercer ReplyOrder2
                   {ReplyOrder2 RequestOrder->ReplyOrder2}))
;;=> #'schema-example.clj-example/order-coercer2
(order-coercer2
  {:id "10" :product "pencil" :qty 5 :unit-price 1.0 :product2 "note" :qty2 5 :unit-price2 3.0})
;;=> {:order-id "10", :ordered-product [{:product-name "pencil", :qty 5, :unit-price 1} {:product-name "note", :qty 5, :unit-price 3}]}
```

## Using core.typed

Here, we will give a brief introduction to core.typed, which is a different approach from prismatic/schema. core.typed is an optional

typed system in Clojure and enables to check types statically for vars, function parameters, and macros at compilation time. It is also possible to annotate protocols and data types.

To start with core.typed, let's modify project.clj as follows and restart REPL:

```
:dependencies [[org.clojure/clojure "1.8.0"]
                [org.clojure/core.typed "0.3.21"]]
:repositories {"sonatype-oss-public"
"https://oss.sonatype.org/content/groups/public/"}
:injections [(require 'clojure.core.typed)
            (clojure.core.typed/install)]
```

Create src/typed_example/core.clj and declare the namespace as follows:

```
(ns typed-example.core
  {:lang :core.typed}
  (:require [clojure.core.typed :as t]))
```

Then, let's define the annotation of my-add using clojure.core.typed/ann:

```
(t/ann my-add [Number Number -> Number])
;;=> nil
```

We will define my-add using clojure.core/defn and test it:

```
(defn my-add [a b] (+ a b))
;;=> #'typed-example.core/my-add
(my-add 1 2)
;;=> 3
```

It works! Now, we will test a failure case to give a string parameter:

```
(my-add 1 "a")
;;=> Type Error (*cider-repl typed-example:49585*:459:20)
Function add could not be applied to arguments:
;;=> Domains:
;;=>     Number Number
;;=> Arguments:
;;=>     (t/Val 1) (t/Val "a")
```
