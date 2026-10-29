To create a vector, enclose its elements with []. To create an empty vector, use [] or (vector):

```
["A Study in Scarlet"
"The Sign of the Four"
"The Hound of the Baskervilles"
"The Valley of Fear"]
;;=> ["A Study in Scarlet" "The Sign of the Four" "The Hound of the Baskervilles" "The Valley of Fear"]
[]
;;=> []
(vector)
;;=> []
```

We can use first, rest, and conj with vectors. Let's begin with first and rest:

```
(first [1 2 3 4 5])
;;=> 1
(first [])
;;=> nil
(rest [1 2 3 4 5])
;;=> (2 3 4 5)
```

The rest of the vector looks like a returning list, but it is not list. It is ChunkedSeq:

```
(class (rest [1 2 3 4 5]))
;;=> clojure.lang.PersistentVector$ChunkedSeq
```

To add elements to a vector, use the conj function. conj appends elements to the vector:

```
(conj [1 2 3 4] 5)
;;=> [1 2 3 4 5]
```

# Maps

Maps are associative lists, and they can be accessed efficiently with keys and return their values.

To create a map, enclose key-value pairs with {}: