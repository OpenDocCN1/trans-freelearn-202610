

```
(gen/not-empty (gen'/string-from-regex #"(B(A|O)M)+")) 1)
; ("BOM")
; try it !
(gen/sample
(gen/not-empty (gen'/string-from-regex #"B(A|O){2}M")) 1)
```

现在，在生成元组时，通常集合的不同值之间存在相关性，这正是 Herbert 库发挥作用的地方。

## 基于模式的随机值

Herbert 生成器将语法和生成器都带入了随机生成器的工具箱中。

### 提示

注意，使用的是不同的命名空间！但它们的使用方式相似，并且稍后将在相同的测试上下文中使用。

要使用 Herbert 生成 int 序列，你将使用 hg/sample 方法，可以像到目前为止我们看到的 test.check 一样使用它。

hg/sample 生成器然后接受一个语法，生成器将根据该语法生成值。

这给出以下内容：

```
(require '[miner.herbert.generators :as hg])
(hg/sample '[int*] 5)
; ([ ] () [2 0] () (-4 -2))
```

甜美的随机世界。

你可能已经对整数随机性感到足够了，所以让我们看看元组：

```
(hg/sample '{:a int :b [sym+] :c str} 3)
```

语法，被引用，是三个键值对的映射，键为 :a :b :c，值是：
- :a 的一个随机整数
- :b 的一个符号向量
- :c 的一个字符串

运行之前的 Herbert 生成器会得到：

```
; {:c "", :b (*mt), :a 0}
; {:c "", :b [_... !.Z _B3.], :a 0}
; {:c "}", :b (.14 .), :a 9223372036854775807})
```

显然，运行两次会得到不同的值，但所有值都符合语法。

另一个例子，这次生成一个更详细的映射：

```
(hg/sample '{:a num :b pos :c neg :d (vec int+ zero+)} 1)
; ({:c -2.0, :b 1, :d [9223372036854775807 0], :a 1.5E-5})
```

你现在已经回顾了如何使用生成器以及如何根据特定模式创建随机值。让我们付诸实践。

## 如何操作...

QuickCheck，基于随机的测试，取我们看到的随机生成器并用于测试。

为此，你需要：
- 编写一个规范，并命名
- 编写采样迭代的次数大小
- 为规范的输入定义随机模式
- 为每个随机值定义一组需要检查的属性

## 你的第一个 test.check 规范

记住这一点，让我们通过第一个规范和第一个示例。让我们导入一些用于属性、生成器以及名为 defspec 的测试辅助函数的命名空间：

```
(ns quickchecks.core
  (:require  [clojure.test.check.properties :as prop])
```

```
(:require [clojure.test.check.generators :as gen])
(:require [clojure.test.check.clojure-test :refer [defspec]])
```

你的第一个规范将如下所示：
- 给定一个名称，`some-ones`
- 设置调用生成器的次数，这里为 5
- 使用 `prop/for-all` 对生成器的所有值验证一个属性
- 总是生成值 1，使用之前看到的 `gen/return`

验证是对生成器值的断言，这里是 1：

```
(defspec some-ones
  5
  (prop/for-all [v (gen/return 1)]
    (= v 1)))
```

我们对这个应该相当有信心，这是一个应该经常通过的测试。

## 自动运行测试

要自动运行测试，一个不错的选择是使用 `midje` 的 `autotest`，使用：

```
lein midje :autotest
```

这会重新加载并重新运行需要的测试命名空间和依赖代码。示例输出如下：

```
================================================================================
===
Loading (quickchecks.core0)
{:result true, :num-tests 5, :seed 1466917660382, :test-var "some-ones"}
>>> Output from clojure.test tests:

0 failures, 0 errors.
>>> Midje summary:
```

请注意，至少通过 println 显示一些示例输出是非常有用的，并且请注意，你刚刚生成了 1,000 条条目的随机化输入，而随机定义相当简单。

这里的最后一个示例检查算术是否仍然有效：

```
(defn my-eq [a b]
  (> 1e10 (- b a)))
(defn my-tan [v]
  (/ (Math/sin v) (Math/cos v)))

(defspec cosintan
  1000
  (prop/for-all [v (gen/elements (range 1 10000))]
    (my-eq (Math/tan v) (my-tan v) )))
```

显然，你也可以使用测试执行类似操作，但必须承认之前的版本可读性很好。

## 生成器和解构

你之前见过 gen' 命名空间。这里，我们将看到来自 test.chuck 的另一个有用的工具，生成器的解构。

之前的示例之一展示了如何使用 apply 处理参数，但这样更容易：

```
...
; 在命名空间定义中
(:require [com.gfredericks.test.chuck.properties :as prop'])
...
(defn my-len [n]
  (count (str n)))

(defspec split-n-spec
  10
  (prop'/for-all [ [ele ele2] (gen/tuple gen/int gen/int)]
    (< 0 (my-len (+ ele ele2)))))
```

请注意 ele 和 ele2 如何被分配给元组的每个值。注意，我们使用的是来自 test.chuck 命名空间的 prop'/for-all，这与通常来自 test.check 的 prop 不同。

## Herbert

从食谱的第一部分，你还记得 Herbert，它允许使用语法生成随机输入。

要将 Herbert 的属性与 defspec 一起使用，你需要从到目前为止看到的内容中进行一些参数体操。

这里检查在前，生成模板在后。

示例生成一个包含两个键值对 :a 和 :b 的映射，并检查 :a 的第一个值的第二个参数中是否包含值为 0 的映射整数：

```
(defspec hello-again
  10
  (hg/property
    (fn [m] (== (get-in m [:a 1 :int]) 0))
    '{:a (vec kw {:int 0}) :b str}))
```

当你想要根据来自外部世界的数据结构生成和拥有大型映射或大型向量输入时，这种技术非常有用。

## 还有更多...

本食谱的最后一部分简要展示了如何将介绍的概念应用于 API 测试，在此上下文中为 compojure 路由。

假设你的 API 中有一组路由；生成随机输入并确保逻辑得到良好封装，并且至少不会破坏应用程序的基础会很棒。

我们将考虑非常简单的路由，其中带有 ID 的请求到达应用程序，应用程序始终返回 "Hello Foo"。

显然，这不是一个能赚取数百万美元的伟大应用程序，

但其中可能有些东西：

```
(def my-routes
  (routes
(GET "/:id" [] "Hello Foo")))
```

路由很简单，但你希望生成随机输入并向这些 compojure 应用发送请求以验证返回结果。

这里有一个简单的实用方法，显示发送到路由函数的映射。正如你注意到的，这只是发送一个包含请求类型、uri、头信息和一些参数的映射：

```
(defn request [uri app & params]
  (app {:request-method :get
        :headers {"content-type" "text/plain"}
        :uri uri
        :params params}))
```

有了这个工具，让我们使用应用程序 my-routes 和实用方法编写一个测试我们应用程序的规范：

```
(defspec ringing
  10
  (prop/for-all [a gen/int]
    (= "Hello Foo"
       (:body (request (str "/" a) my-routes [])))))
```

生成器为你提供一个随机 int，然后用于创建请求的 uri。最后，我们确保应用程序返回的结果非常稳定，始终返回友好的 Foo 问候语。

第一个示例相当静态，那么如果想要实际动态生成路由怎么办？

它几乎完全相同，唯一的新东西是使用 let 块在测试范围内绑定路由：

```
(defspec ringing-2
  100
  (prop'/for-all [a gen/int]
```

```
(let [local-routes
    (routes  (GET "/:id" [] (str "Hello Foo" a)))]
  (= (str "Hello Foo" a) (:body (request (str "/" a) local-routes []))))
```

有了这个食谱，你已经准备就绪！享受大量随机测试吧！

