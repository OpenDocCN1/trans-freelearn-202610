```
(apply-fn tc "key-1" inc)))
;;=> "Elapsed time: 2467.011652 msecs"
;;=> nil
```

Let's check "key-1" either in REPL-1 or REPL-2. It should be 20,000:

```
(c/get tc "key-1")
;;=> 17258
```

Wrong! There is a concurrency problem.

So let's do the same test using the CAS function. We will learn how CAS works. We will run the following code either in REPL-1 or REPL-2.

First of all, let's see how CAS works using a simple code.

clojurewerkz.spyglass.client/gets gets a current value and clojurewerkz.spyglass.client/cas updates the value using CAS. If the CAS update is successful, cas returns :ok. If it fails, it returns :exists.

Let's set "key-1" to 0 and get the CAS value for it:

```
(c/set tc "key-1" 0 0)
;;=> #clojurewerkz.spyglass.OperationFuture[{:status :ready, :val true} 0x7e0e69e5]
  (def cas-value (:cas (c/gets tc "key-1")))
  ;;=> 80010
  #'user/cas-value
```

The CAS value is 80010 in the preceding case. Then we will update "key-1" using the cas function:

```
(c/cas tc "key-1" cas-value 1)
;;=> ok
(c/get tc "key-1")
;;=> 1
```

The preceding CAS operation updates the CAS value and it fails to update using the previous CAS value:

```
(:cas (c/gets tc "key-1"))
;;=> 80011
```