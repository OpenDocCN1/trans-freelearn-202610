including value-based equality and hashCode.

Record implements a lot of Clojure interfaces, whereas type implements only the clojure.lang.IType:

```
(supers Author)
;;=> #{clojure.lang.IKeywordLookup clojure.lang.ILookup clojure.lang.Seqable java.lang.Object java.lang.Iterable clojure.lang.IMeta clojure.lang.IHashEq clojure.lang.IPersistentMap clojure.lang.Counted java.io.Serializable java.util.Map clojure.lang.IPersistentCollection clojure.lang.Obj clojure.lang.Associative clojure.lang.IRecord}
(supers Point3D)
;;=> #{java.lang.Object clojure.lang.IType}
(def x (Point. 1 1))
;;=> #'chapter03.protocols/x
(def y (Point. 1 1))
;;=> #'chapter03.protocols/y
(= x y)
;;=> true
```

Record supports a value-based equality and hashCode. See the following code:

```
(defrecord PointRecord [x y])
;;=> chapter03.protocols.PointRecord
(= (PointRecord. 10 5)
   (PointRecord. 10 5)
   )
;;=> true
(deftype PointType [x y])
;;=> chapter03.protocols.PointType
(= (PointType. 10 5)
   (PointType. 10 5)
   )
;;=> false
```

Records are immutable and persistent. Clojure's record implements the clojure.lang.IPersistentMap, we can access records using functions for map. Fields of types can be mutable. We will show a technique for this later in this recipe.