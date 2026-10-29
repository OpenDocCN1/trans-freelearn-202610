```
(let
  [{:syms [x y z]} {'x 1 'y 2  'z  3}]
  [x y z])
;;=> [1 2 3]
```

If you would like to specify the default values, use the :or keyword and specify the default value. In the following example, the values of :y and :z are not specified in the map and as a result, the y and z bind to the default values:

```
(let
  [{:keys [x y z] :or {x 0 y 0 z 0}}
    {:x 1}
  ]
  [x y z])
;;=> [1 0 0]
```

Using the :as keyword, the followed variable binds to the map itself. In the following example, the all binds to {:x 1 :a 2 :b 3}:

```
(let
  [{:keys [x y z] :or {x 0 y 0 z 0} :as all} {:x 1 :a 2 :b 3}
  ]
  [x y z all])
;;=> [1 0 0 {:x 1,  :a 2,  :b 3}]
```

The following map-destructuring code extracts values from a nested map:

```
(let
  [{x :x y :y {a :a b :b} :z}
    {:x 1 :y 2 :z {:a 3 :b 4}}
  ]
  [x y a b])
;;=> [1 2 3 4]
```

The following code uses the shortcut style of nested-map destructuring:

```
(let
  [{:keys [x y] {:keys [a b]} :z}
    {:x 1 :y 2 :z {:a 3 :b 4}}
  ]
  [x y a b])
:;=> [1 2 3 4]
```