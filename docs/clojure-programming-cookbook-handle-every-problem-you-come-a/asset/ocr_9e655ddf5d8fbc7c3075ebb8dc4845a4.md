```
(import 'scala.math.BigInt)
;;=> scala.math.BigInt
(import 'java.math.BigInteger)
;;=> java.math.BigInteger
(.factorial calc (BigInt. (BigInteger. "25")))
;;=> 15511210043330985984000000
```

Calling factorial is a bit complicated. The code converts to Scala's scala.math.BigInt.

# Calling singleton methods

Calling Scala's singleton methods is the same as calling Java's static methods:

```
(import 'chapter05.Main)
;;=> chapter05.Main
(Main/hello "Makoto")
;;=> "hello from Scala: Makoto !"
(import 'chapter05.MyTuple)
;;=> chapter05.MyTuple
(def tuple1 (MyTuple/tuple 1 5))
;;=> #'clojure-scala.core/tuple1e
```

# Accessing tuples

To access tuples, use _1, _2, and so on:

```
(. _1 tuple1)
;;=> 1
(. _2 tuple1)
;;=> 5
```

# Accessing Scala fields

Let's access Scala fields from Clojure. In Scala, declaring a field generates accessor methods automatically, but method names are different from Java:

```
package chapter05
import scala.beans.BeanProperty

class Student {
  var name = new String("")
  var address = new String("")
```