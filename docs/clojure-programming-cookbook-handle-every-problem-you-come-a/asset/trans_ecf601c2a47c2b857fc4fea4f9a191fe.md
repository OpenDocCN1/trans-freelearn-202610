

get 通过键查找映射条目并返回其值。如果 get 在集合中找不到该键，则返回 nil。

如果在这种情况下你不想得到 nil，请在键不存在时指定想要的值：

```
(get {:a 1, :b 2, :c 3 :d 4 :e 5} :c)
;;=> 3
(get {:a 1, :b 2, :c 3 :d 4 :e 5} :f)
;;=> nil
(get {:a 1, :b 2, :c 3 :d 4 :e 5} :f :not-found)
;;=> :not-found
```

assoc 向映射添加键值对。如果映射中已存在键值对，则替换该值：

```
(assoc {:a 1, :b 2, :c 3 :d 4} :e 5)
;;=> {:a 1, :b 2, :c 3, :d 4, :e 5}
(assoc {:a 1, :b 2, :c 3 :d 4} :c 5)
;;=> {:a 1, :b 2, :c 5, :d 4}
```

assoc 可以使用多个键值对作为其参数：

```
(assoc {:a 1, :b 2, :c 3 :d 4} :e 5 :f 6)
;;=> {:a 1, :b 2, :c 3, :d 4, :e 5, :f 6}
```

dissoc 从映射中移除键值对：

```
(dissoc {:a 1, :b 2, :c 3 :d 4} :c)
;;=> {:a 1, :b 2, :d 4}
(dissoc {:a 1, :b 2, :c 3 :d 4} :c)
;;=> {:a 1, :b 2, :d 4}
(dissoc {:a 1, :b 2, :c 3 :d 4} :e)
;;=> {:a 1, :b 2, :c 3, :d 4}
```

## Sets

集合维护其元素的唯一性。要创建集合，以哈希 (#) 开头，然后用 {} 括住元素。如果要创建空集合，使用 #{} 或 (hash-set)：

```
#{"apple" "orange" "banana" "peach" "strawberry"}
;;=> #{"peach" "apple" "banana" "orange" "strawberry"}
```

```
#{}
;;=> #{}
(hash-set)
```

我们可以使用 `first`、`rest` 和 `conj` 处理集合。集合不会保持你输入数据的顺序，因此 `first` 或 `rest` 的结果可能与你预期不同：

```
(first #{:a :b :c :d :e})
;;=> :e
(rest #{:a :b :c :d :e})
;;=> (:c :b :d :a)
(conj #{:a :b :c :d} :e)
;;=> #{:e :c :b :d :a}
(conj #{:a :b :c :d :e} :e)
;;=> #{:e :c :b :d :a}
```

`contains?` 函数检查元素是否存在于集合中，而 `disj` 则从集合中消除该元素：

```
(contains? #{:a :b :c :d :e} :c)
;;=> true
(contains? #{:a :b :c :d :e} :f)
;;=> false
(disj #{:a :b :c :d :e} :c)
;;=> #{:e :b :d :a}
(disj #{:a :b :c :d :e} :c :d)
;;=> #{:e :b :a}
```

## How it works...

Clojure 中的所有集合类型都是持久的且不可变的。持久数据结构在被修改时会保留其自身之前的版本。Clojure 对集合的修改版本有高效的实现，因此它不会简单地复制整个数据结构，而是共享原始数据。

## Clojure collections

所有集合类型都实现了 `clojure.lang.Seqable` 接口，其 `seq` 方法返回 `clojure.lang.ISeq`。`ISeq` 具有 `cons`、`first`、`more` 和 `next` 函数：

```
public interface Seqable {
    ISeq seq();
}
```

```
public interface ISeq extends IPersistentCollection {
    Object first();
    ISeq next();
    ISeq more();
    ISeq cons(Object o);
}
```

ISeq 实现了 IPersistentCollection，它具有 count、cons、empty 和 equiv：

```
public interface IPersistentCollection
    extends Seqable {
    int count();
    IPersistentCollection cons(Object o);
    IPersistentCollection empty();
    boolean equiv(Object o);
}
```

## Differences between lists and vectors

列表和集合表示元素序列，但存在一些差异：

- 列表是 `clojure.lang.PersistentList`，向量是 `clojure.lang.PersistentVector`。
- 列表以链表实现。向量使用树结构实现，每个节点有 32 个元素。
- 向量对索引访问高效。列表的索引访问为 O(n)，而向量的索引访问为 O(log32(n))。
- 这就是为什么，使用 `conj` 时，列表和向量的结果顺序不同。`conj` 将元素放在列表开头，但将元素放在向量末尾：

```
(conj '(2 3 4 5) 1)
;;=> (1 2 3 4 5)
(conj [1 2 3 4] 5)
;;=> [1 2 3 4 5]
```

以下代码测试列表和向量的索引访问。此测试测量对包含 100 万个元素的集合中最后一个元素的访问。测试循环 100 次。访问列表需要半秒，但访问向量仅需 0.1 毫秒：

```
(let [n 1000000 iter 100
      l (into () (repeat n "a"))
      v (into [] (repeat n "a"))
      ]
  (time (dotimes [_ iter] (nth l (dec n) )))
  (time (dotimes [_ iter] (nth v (dec n) )))
)
"Elapsed time: 532.623841 msecs"
"Elapsed time: 0.167739 msecs"
```

反转向量的顺序比反转列表的顺序更快：

```
(do
  (time (reverse (into '() (range 100000))))
  (time (reverse (into [] (range 100000))))
  nil
)
"Elapsed time: 60.67435 msecs"
"Elapsed time: 26.431954 msecs"
nil
```

原因在于 clojure.lang.PersistentVector 实现了 clojure.lang.Reversible，但 clojure.lang.PersistentList 没有。

## Clojure is immutable

由于 Clojure 集合的函数是不可变的，因此它不会修改原始集合。这与 Python 或 Ruby 不同。在以下 Python 代码中，insert 修改了原始列表：

```
l = [2, 3, 4, 5]
l.insert(0, 1)
l
#[1, 2, 3, 4, 5]
```

下面的 Ruby 代码也修改了原始列表：

```
$ irb
irb(main):001:0> letters = Array.new([2,3,4])
=> [2, 3, 4]
irb(main):002:0> letters.insert(0, 1)
```

```
=> [1, 2, 3, 4]
```

但 Clojure 不会更改原始数据：

```
(def l '(2 3 4 5))
#'collection.core/l
(conj '(2 3 4 5) 1)
;;=> (1 2 3 4 5)
l
;;=> (2 3 4 5)
```

使用不可变数据可确保在从其他函数调用返回后参数保持未修改。另一方面，调用函数会导致参数被意外修改，从而导致难以找到的错误。

## There's more...

在这里，我们将向你展示一些操作集合的技术。

## Converting collections between different types

into 在集合中转换类型。以下代码在向量和列表之间转换：

```
(into [] '(1 2 3 4 5))
;;=> [1 2 3 4 5]
(into '() [1 2 3 4 5])
;;=> (5 4 3 2 1)
```

在以下代码中，into 在映射和向量之间转换：

```
(into {} [[:a 1][:b 2] [:c 3][:d 4][:e 5]])
;;=> {:a 1, :b 2, :c 3, :d 4, :e 5}
(into [] {:a 1 :b 2 :c 3 :d 4 :e 5})
;;=> [[:a 1] [:b 2] [:c 3] [:d 4] [:e 5]]
```

最后，这个 into 在集合和向量之间转换：

```
(into [] #{:a :b :c :d :e})
;;=> [:e :c :b :d :a]
(into #{} [:a :b :c :d :e])
;;=> #{:e :c :b :d :a}
```

## Using distinct

distinct 类似于使用集合的 `into`。它消除重复元素：

```
(distinct ["apple" "orange" "melon" "grape" "orange" "pinapple"])
;;=> ("apple" "orange" "melon" "grape" "pinapple")
(into #{} ["apple" "orange" "melon" "grape" "orange" "pinapple"])
;;=> #{"apple" "pinapple" "orange" "grape" "melon"}
```

与 `set` 不同，使用 `distinct` 不会更改数据类型。

## Accessing and updating elements from collections

在本教程中，我们将教你如何访问集合中的元素并更新元素。

## Getting ready

你只需要 REPL，如第 1 章《使用 Clojure 进行实时编程》中的教程所述，且不需要额外的库。启动 REPL，以便你可以查看本教程中的示例代码。

## How to do it...

让我们从访问集合开始。

## Accessing collections using the nth function

nth 从集合中获取第 n 个元素。nth 的第二个参数从 0 开始，如果第二个参数大于元素数量减 1，则抛出异常：

```
(nth [1 2 3 4 5] 1)
;;=> 2
(nth '("a" "b" "c" "d" "e") 3)
;;=> "d"
(nth [1 2 3] 3)
;;=> IndexOutOfBoundsException
clojure.lang.PersistentVector.arrayFor
(PersistentVector.java:153)
```

如果你想避免此类异常，请使用第三个参数作为返回值：

```
(nth [1 2 3] 3 nil)
;;=> nil
```

请注意，nth 不适用于映射和集合。

## Accessing maps or sets using get

get 使用键访问映射和集合，如果映射或集合中存在对应的键，则返回其值。如果映射或集合中没有相同的键，则返回 nil 或第三个参数：

```
(get {:a 1 :b 2 :c 3 :d 4 :e 5} :c)
;;=> 3
(get {:a 1 :b 2 :c 3 :d 4 :e 5} :f)
;;=> nil
(get {:a 1 :b 2 :c 3 :d 4 :e 5} :f :not-found)
;;=> :not-found

(get #{:a :b :c} :c)
;;=> :c
(get #{:a :b :c} :d)
;;=> nil
(get #{:a :b :c} :d :not-found)
;;=> :not-found
```

## Maps, sets, and keywords are functions to access collections

将映射或集合用作第一个参数是惯用的 Clojure 方法。以下代码与使用 get 相同：

```
;;=> :not-found
```

使用关键字也是 Clojure 惯用的。这与 get 相同：

```
(:c {:a 1 :b 2 :c 3 :d 4 :e 5})
;;=> 3
(:f {:a 1 :b 2 :c 3 :d 4 :e 5})
;;=> nil
(:f {:a 1 :b 2 :c 3 :d 4 :e 5} :not-found)
;;=> :not-found
```

get 对于映射，如果键存在于元素中，则返回该键。如果没有匹配的键，则返回 nil：

```
(get #{:banana :apple :strawberry :orange :melon} :orange)
;;=> :orange
```

