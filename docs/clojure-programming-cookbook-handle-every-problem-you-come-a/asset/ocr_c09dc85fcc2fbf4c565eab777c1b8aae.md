Arrays in Java are not immutable, so we can change the value of the element using aset:

```
(aset str-array 0 "x")
;;=> "x"
(aget str-array 0)
;;=> "x"
```

long-array creates arrays whose elements are the long type of Java primitives. We will compare the performance with an array whose elements are Java's long objects:

```
(time
  (let [n 10000000
        a (long-array (range n))]
    (reduce + a)))
;;=> "Elapsed time: 1339.504072 msecs"
;;=> 49999995000000
(time
  (let [n 10000000
        a (into-array (range n))]
    (reduce + a)))
;;=> "Elapsed time: 6101.370939 msecs"
;;=> 49999995000000
```

The following code creates Java's byte-array:

```
(byte-array (map int [\C \l \o \j \o \u \r \e]))
;;=> #object["[B" 0x590f9b92 "[B@590f9b92"]
(String. (byte-array (map int [\C \l \o \j \o \u \r \e])))
;;=> "Clojoure"
```

The following code is to create a two-dimensional array:

```
(def mat1 (make-array Double/TYPE 10 10))
;;=> #'chapter05.calling-java/mat1
(aget mat1 9 9)
;;=> 0.0
```

To specify, Double/TYPE generates an array whose elements are double primitive types.

# How it works...