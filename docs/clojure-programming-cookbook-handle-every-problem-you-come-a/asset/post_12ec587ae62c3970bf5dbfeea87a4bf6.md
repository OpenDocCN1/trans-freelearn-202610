
The ClojureScript file will reference this later on, so your new code can simply reference the Clojure code:

```
(ns example.hello
  (:require
    [example.shared :as shared]))
(js/alert (take 5 shared/fib-seq))
```

To compile all this, just make sure you have separate folders for ClojureScript files and regular Clojure files. This is done in project.clj by adding where the Clojure files are located:

```
:source-paths ["src-clj"]
```

And telling the ClojureScript compiler where the cljs file are:

```
:builds {
  :dev
  {:source-paths ["src-cljs"]
   :compiler {:output-to "out/seven.js"
              :output-dir "out"
              :optimizations :none}}}
```

![](img/d7b631352a782ee50160cf8eb995152f_367_0.png)

Et voila. All your Clojure-based algorithms can be reused for the web! Now it's your turn to go and explore.

## Chapter 6. Concurrency and Parallelism

In this chapter, we will cover the following topics:

- Solving concurrent problems using Clojure
- Distributed actor-based dev with Akka
- Using Spyglass and Couchbase to share state between JVMs
- Reactive programming with meltdown
- Bridging core.async
- On Quasar/Pulsar

### Introduction

In this chapter, we will learn about the concurrency and parallelism that Clojure provides. First, we will take a look at concurrency and parallelism features. Next, we will try to use Scala's Akka library and Spyglass. Then we will learn Quasar/Pulsar, which are fibers, channels, and actors for Clojure and Java used in Netflix. Finally, we will have a look at meltdown, a nice Clojure wrapper for Reactor, which is a toolkit for asynchronous programming, event passing, and stream processing.

## Solving concurrent problems using Clojure

In this recipe, we will learn Clojure's atoms, refs, and agents. Then we will have a look at Clojure's parallel features.

### Getting ready

In this recipe, we will not use an external library. So, the only necessary thing is to start REPL to run code.

### How to do it...

Clojure provides solutions for the concurrency problem. Clojure's vars defined in `def` are ensured only in single thread and are not safe in concurrent environments. Clojure provides three types of resources for concurrent problem. Here, we will introduce you atoms, refs, and agents that work in concurrent environments.

### Using atoms

Atoms provide a way to manage shared, synchronous, and independent states.

#### Creating and referring atom

The `atom` function sets the initial value to atom. The following code initializes the atom to 1 and sets the atom to the var x:

```
(def x (atom 1))
;;=? #'chapter06.concurrency/x
```

So, the value of x binds to Atom instance:

```
x
;;=> #<Atom@541e8f8d: 1>
```

To see the values of the atoms, use the `deref` macro or the reader macro

```
@:

(deref x)
;;=> 1
@x
;;=> 1
```

#### Updating atom

There are a couple of ways to update atoms. The swap atomically applies a function to the original value and sets the atom to it:

```
(swap! x inc)
;;=> 2
@x
;;=> 2
(swap! x (partial + 1))
;;=> 3
```

The following image shows how the atom value changes by swap:

![](img/d7b631352a782ee50160cf8eb995152f_370_0.png)

The reset! function sets the value of atom regardless of the current value of atom:

```
(reset! x 1)
;;=> 1
x
;;=> #<Atom@541e8f8d: 1>
```

#### Using validator

There is a validation mechanism for atom. Atom's validators check values of atoms. The following code checks the atom value should be less than 5:

```
(def y (atom 1 :validator (partial > 5)))
;;=> #'chapter06.concurrency/y
(swap! y (partial + 2))
;;=> 3
(swap! y (partial + 2))
;;=> IllegalStateException Invalid reference state
clojure.lang.ARef.validate (ARef.java:33)
```

#### Using CAS operation

**Compare-and-swap (CAS)** is an atomic instruction used in concurrent environments and achieves synchronization between race conditions. In Clojure, the `compare-and-set!` function updates the value of the `new-value` atom if the current value of the atom equals the `old-value`. The syntax is as follows:

```
(compare-and-set! atom old-value new-value)
```

The next function we define gets the current value of the atom, sleeps for the interval, and updates the value of atom using the `compare-and-set!`:

```
(defn cas-test! [my-atom interval]
  (let [v @my-atom u (inc @my-atom)]
    (println "current value = " v ", updated value = " u)
    (Thread/sleep interval)
    (println "updated " (compare-and-set! my-atom v u))))
;;=> #'chapter06.concurrency/cas-test!
```

Let's define an atom and see how `compare-and-set!` works:

```
(def x (atom 1))
;;=> #'chapter06.concurrency/x
```

The following test code updates twice by CAS operations, but both updates are successful since two updates occur sequentially and no race condition occurs:

```
```clojure
(do
  (cas-test! x 20)
  (cas-test! x 30))
;;=> current value = 1 , updated value = 2
;;=> updated true
;;=> current value = 2, updated value = 3
;;=> updated true
```
```

The following diagram depicts how two updates change the value of x:

![](img/d7b631352a782ee50160cf8eb995152f_372_0.png)

In the following code, concurrent updates cause a race condition, and the later update fails since the original value has been updated by the other:

```
```clojure
(do
  (def x (atom 1))
  (future (cas-test! x 20))
  (future (cas-test! x 30)))
;;=> current value = 1 , updated value = 2
;;=> current value = 1 , updated value = 2
;;=> updated true
;;=> updated false
```
```

In the following diagram, two updates make a race condition, and the later update fails because the earlier update changes the value of the x:

![](img/d7b631352a782ee50160cf8eb995152f_373_0.png)

## Software Transactional Memory using ref and dosync

Software Transactional Memory (STM) provides transactional accesses to multiple resources on memory. STM coordinates multiple resources in a transaction and provides consistency, atomicity, and isolation. Inside dosync, resources defined by ref are coordinated.

Let's look at how STM works in Clojure. In the following example code, we use the ref, dosync, and alter functions for STM transactions.

#### Creating ref

The ref function creates a ref with an initial value. makoto-account and nico-account exist. They are credit balances for two customers. The ref function creates a ref, whose value is a map:

```
(def makoto-account (ref {:name "Makoto Hashimoto" :amount 1000}))
;;=> #'chapter06.concurrency/makoto-account
(def nico-account (ref {:name "Nicolas Modrzyk" :amount 2000}))
;;=> #'chapter06.concurrency/nico-account
```

#### Updating refs using alter

Then, the following transfer! function executes a transaction moving the amount of money between the from account and the to account:

```
(defn transfer! [from to amount]
  (dosync
    (println "transfer money from " (:name @from) " to " (:name @to) " amount = " amount " begins")
    (alter from assoc :amount (- (:amount @from) amount))
    (Thread/sleep 500)
    (alter to assoc :amount (+ (:amount @to) amount))
    (println "Now, " (:name @from) " amount is " (:amount @from) " and " (:name @to) " amount is " (:amount @to))
    ))
;;=> #'chapter06.concurrency/transfer!
```

There are a couple of alter calls inside the dosync in the transfer! function that form a transaction. Let's test to transfer 10 dollars from Makoto to Nicolas:

```
(transfer! makoto-account nico-account 10)
;;=> transfer money from Makoto to Nicolas amount = 10 begins
::=> Now, Makoto amount is 990 and Nicolas amount is 2010
;;=> nil
@nico-account
;;=> {:name "Nicolas Modrzyk", :amount 2010}
(deref makoto-account)
;;=> {:name "Makoto Hashimoto", :amount 990}
```

Great! It works correctly.

Then, let's run two transactions concurrently and see how they work. The first transaction transfers 200 dollars from Makoto to Nico and the second transaction transfers 300 dollars in the same direction:

```
(do
  (future
    (transfer! makoto-account nico-account 200))
  (future
    (transfer! makoto-account nico-account 300)))
;;=> transfer money from Makoto Hashimoto to Nicolas Modrzyk amount = 200 begins
;;=> #<Future@473dad05: :pending>
```
