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