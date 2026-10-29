

(get #{:banana :apple :strawberry :orange :melon} :grape)
;;=> nil
(get #{:banana :apple :strawberry :orange :melon} :grape :not-found)
;;=> :not-found

我们可以使用集合或关键字作为第一个参数，但在这个集合中有一个第三个参数：

```
(#{:banana :apple :strawberry :orange :melon} :orange)
;;=> :orange
(#{:banana :apple :strawberry :orange :melon} :grape)
;;=> nil
(:orange #{:banana :apple :strawberry :orange :melon})
;;=> :orange
(:grape #{:banana :apple :strawberry :orange :melon})
;;=> nil
```

## 使用 second、next、ffirst 和 nfirst 访问集合

second 从集合中返回第二个元素：

```
(second [1 2 3 4 5])
;;=> 2
(second '())
;;=> nil
```

next 的行为几乎与 rest 相同，但当结果为空时 next 返回 nil：

```
(next [1])
;;=> nil
(rest [1])
;;=> ()
```

ffirst 返回第一个元素的第一个元素：

```
(ffirst [[1 2 3] 4 [3 5 6]])
;;=> 1
(ffirst {:a 1 :b 2})
;;=> :a
```

这等价于以下代码：

```
(first (first [[1 2 3] 4 [3 5 6]]))
```

```
;;=> 1
(first (first {:a 1 :b 2}))
;;=> :a
```

nfirst 返回第一个元素的下一个元素：

```
(nfirst [[1 2] [3 4][5 6]])
;;=> (2)
(nfirst {:a 1 :b 2})
;;=> (1)
```

## 使用 update 更新集合

update 使用第三个元素指定的函数更新匹配的值：

```
(update {:a 1 :b 2 :c 3} :a inc)
;;=> {:a 2, :b 2, :c 3}
```

## 它是如何工作的...

映射、集合和向量实现了 clojure.lang.IFn。Clojure 函数如 + 实现了 IFn。这就是映射和集合可以作为函数的原因。ifn? 测试参数是否实现了 clojure.lang.IFn：

```
(ifn? +)
;;=> true
(ifn? [])
;;=> true
(ifn? {})
;;=> true
(ifn? #{})
;;=> true
(ifn? :a)
;;=> true
(ifn? '())
;;=> false
(ifn? 1)
;;=> false
```

向量和映射是函数，但列表和整数不是函数。

## 还有更多...

在这里，我们将教你一些用于访问和更新集合的有用函数。

## 使用 get 获取向量

与映射和集合一样，get 也适用于向量：

```
(get ["a" "b" "c" "d" "e"] 3)
;;=> "d"
(get ["a" "b" "c" "d" "e"] 5)
;;=> nil
(get ["a" "b" "c" "d" "e"] 5 :not-found)
;;=> :not-found
```

向量也是函数：

```
(["a" "b" "c" "d" "e"] 3)
;;=> "d"
(["a" "b" "c" "d" "e"] 5)
;;=> IndexOutOfBoundsException
clojure.lang.PersistentVector.arrayFor
(PersistentVector.java:153)
```

使用 get 获取列表始终返回 nil。不要那样做：

```
(get '(1 2 3) 1)
;;=> nil
```

## 使用集合作为映射的键

与 Java、Python 和 Ruby 不同，Clojure 可以使用集合类型作为键。下一个映射类型使用映射作为键，并使用映射键查找值：

```
(def location
  {{:x 1 :y 1} "Nico" {:x 1 :y 2} "John" {:x 2 :y 1} "Makoto"
   {:x 2 :y 2} "Tony"}
)
;;=> #'chapter03.core/location
(location {:x 2 :y 2})
;;=> "Tony"
```

当键是空间维度时，这很有用。

## 使用 get-in

get-in 将给定的键与集合关联起来，并在向量中返回匹配元素的值。get-in 将第一个参数作为集合，第二个参数通常是多个键的向量，您想要查找这些键。我们将回到 Conan-Doyle 的传记，看看 get-in 是如何工作的：

```
(def biography-of-conan-doyle
  {:name "Arthur Ignatius Conan Doyle"
   :born "22-May-1859"
   :died "7-July-1930"
   :occupation ["novelist" "short story writer" "poet" "physician"]
   :nationality "scottish"
   :citizenship "United Kingdom"
   :genre ["Detective fiction", "fantasy", "science fiction", "historical novels", "non-fiction"]
   :notable-works ["Stories of Sherlock Holmes" "The Lost World"]
   :spouse ["Louisa Hawkins" "Jean Leckie"]
   :no-of-children 5
   })
(get-in biography-of-conan-doyle [:genre 2])
;;=> "science fiction"
```

在前面的示例中，get-in 的第二个参数是 [:genre 2]。get-in 在 biography-of-conan-doyle 的 var 中查找 :genre 并找到如下值：

```
["Detective fiction" "fantasy" "science fiction" "historical novels" "non-fiction"]
```

然后它查找向量中的第三个元素并返回 "science fiction"。以下代码是等价代码，返回相同结果：

```
(get (get biography-of-conan-doyle :genre) 2)
;;=> "science fiction"
```

## 使用 assoc-in

assoc-in 与 get-in 类似。它返回一个用第三个参数替换匹配值的集合。如果没有匹配的键，assoc-in 会在新位置插入新值：

```
(assoc-in {:a {:b 1 :c 2} :d 3} [:a :c] 1)
;;=> {:a {:b 1, :c 1}, :d 3}
(assoc-in {:a {:b 1 :c 2} :d 3} [:a :d] 1)
;;=> {:a {:b 1, :c 2, :d 1}, :d 3}
```

## 使用 update-in

update-in 与 get-in 类似，但它只更新现有数据。它还不同之处在于 update-in 的第三个参数是一个函数。

在下一个示例中，第一个 update-in 表达式递增匹配的值。第二个表达式使用 constantly 函数设置常量值 10：

```
(update-in {:a {:b 1 :c 2} :d 3} [:a :c] inc)
;;=> {:a {:b 1, :c 3}, :d 3}
(update-in {:a {:b 1 :c 2} :d 3} [:a :c] (constantly 10))
;;=> {:a {:b 1, :c 10}, :d 3}
```

## 从集合中过滤元素

本节快速展示如何选择 Clojure 序列中的某些元素。

## 准备工作

食谱的前几个部分不需要任何特殊准备，但最后一部分需要在您的 project.clj 文件中添加 core.async 库（或您使用的任何其他依赖管理）：

```
:dependencies [
  [org.clojure/clojure "1.8.0"]
  [org.clojure/core.async "0.2.371"]]
```

## 如何操作...

有四个主要函数用于过滤元素：
- filter
- keep 和 keep-indexed
- remove
- take 和 take-while

让我们通过几个简单的示例。

## 过滤三的倍数

以下代码过滤三的倍数：

```
(filter
  #(= 0 (rem % 3))
  (range 1 10))
; (3 6 9)
```

## 过滤映射的项目

这会过滤映射中的键，该映射在这里用作函数：

```
(filter
  {:b 2 :c 3}
  [:a :b])
; (:b)
```

## 过滤非 nil 值

这会过滤非 nil 值：

```
(filter #(not (nil? %))
  '(nil 1 2 3))
```

## 从序列中移除奇数值

这里，我们生成五个小于 100 的随机偶整数。remove 确实会移除所有奇数元素：

```
(take 5
      (remove
        odd?
        (repeatedly #(rand-int 100))))
; (56 54 86 62 52)
```

## 使用 keep

keep 首先对集合的每个项目应用一个函数，然后保留所有非 nil 值：

```
(keep
even?
(range 10 20))
; (true false true false true false true false true false)
```

## 使用 keep-indexed

keep-indexed 与 keep 类似，但允许您对索引和项目本身同时设置条件。在以下示例中，我们在索引为偶数时保留项目，然后返回项目并进行轻微转换。就像 keep 一样，keep-indexed 只保留作为第一个参数传递的函数中的非 nil 值：

```
(defn mien [ index item ]
  (if (even? index)
    (inc item)
    nil)
)
```

```
(keep-indexed
  mien
  (range 10 20))
```

## 还有更多...

过滤是一个非常有用的 Clojure 函数，可以在许多不同的编码情况下使用。

## 过滤作为转换器

`filter` 函数在没有参数给出时也可以用作转换器。我们将在下一章看到这一点，但这里是快速回顾如何使用此功能。

在以下示例中，`filter` 和 `map` 都用于创建转换器：

```
(transduce
  (filter even?)  conj (range 5))
; [0 2 4]
```

```
(transduce
  (map inc)
  conj
  (range 5))
```

## 使用 core.async 过滤

从最近开始，`filter` 函数可以直接与 `core.async` 库的通道一起使用。这使得在流级别直接应用过滤变得非常有趣。这也是通过转换器完成的，如下所示。

我们需要该库：

```
(require '[clojure.core.async :as async])
```

我们创建通道以及如何过滤通过其中的奇数值：

