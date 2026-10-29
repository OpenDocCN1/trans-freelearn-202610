

```
(defrecord RecordLocation [x y z])
;;=> performance_example.performance_tips.RecordLocation
(deftype TypeLocation [x y z])
;;=> performance_example.performance_tips.TypeLocation
```

然后，我们将定义 map-location、record-location 和 type-location：

```
(def map-location {:x 0.0 :y 0.0 :z 0.0})
;;=> #'performance-example.performance-tips/map-location
(def record-location (->RecordLocation 0.0 0.0 0.0))
;;=> #'performance-example.performance-tips/record-location
(def type-location (->TypeLocation 0.0 0.0 0.0))
;;=> #'performance-example.performance-tips/type-location
```

下面让我们定义 ILocation 协议并为 map、record 和 type 扩展协议：

```
(defprotocol ILocation
  (move [self x y z]))
;;=> ILocation
(extend-protocol ILocation
  clojure.lang.PersistentArrayMap
  (move [self x y z]
    {:x (+ (map-location :x) x)
     :y (+ (map-location :y) y)
     :z (+ (map-location :z) z)})
  RecordLocation
    (move [self x y z]
      (->RecordLocation
        (+ (:x self) x) (+ (:y self) y) (+ (:z self) z)))
  TypeLocation
    (move [self x y z]
      (->TypeLocation
        (+ (.-x self) x) (+ (.-y self) y) (+ (.-z self) z))))
;;=> nil
```

现在，让我们测量它们的性能。首先，我们将测试 map 的 move 方法：

```
(with-progress-reporting
  (bench    (move map-location 2.0 3.0 2.0)))
```

结果如下：

Evaluation count : 386864340 in 60 samples of 6447739 calls.
Execution time mean : 147.959163 ns
Execution time std-deviation : 6.447521 ns
Execution time lower quantile : 139.499802 ns ( 2.5%)
Execution time upper quantile : 162.371977 ns (97.5%)
Overhead used : 9.727097 ns

# 然后，我们将测试 record：

```
(with-progress-reporting
  (bench (move record-location 2.0 3.0 2.0)))
```

# 我们得到了更好的结果：

Evaluation count : 455571960 in 60 samples of 7592866 calls.
Execution time mean : 121.687435 ns
Execution time std-deviation : 3.484611 ns
Execution time lower quantile : 117.866370 ns ( 2.5%)
Execution time upper quantile : 128.063522 ns (97.5%)
Overhead used : 9.727097 ns

# 最后，我们将测试 type：

```
(with-progress-reporting
  (bench (move type-location 2.0 3.0 2.0)))
```

# 这比前两种更好：

Evaluation count : 590518980 in 60 samples of 9841983 calls.
Execution time mean : 93.307679 ns
Execution time std-deviation : 2.608460 ns
Execution time lower quantile : 90.346986 ns ( 2.5%)
Execution time upper quantile : 99.742274 ns (97.5%)
Overhead used : 9.727097 ns

# 以下是基准测试的汇总表：

| Data type | Time (ns) |
|-----------|-----------|
| Map       | 147       |
| Record    | 121       |
|           |           |

# 原始数组

让我们考虑在 Clojure 中使用原始数组。下面的代码是使用 vector 计算 2 + 2^2 + 3^2 + ... + n ^2：

```
(def array1 (into [] (repeatedly 1000 #(rand 10))))
;;=> #'performance-example.performance-tips/array1
(defn my-calc1[ar]
  (reduce +
    (map (fn [x] (* x x))
      ar)))
;;=> #'performance-example.performance-tips/my-calc1
```

我们将定义 array2，它是一个 double 的原始数组：

```
(def array2 (double-array (repeatedly 1000 #(rand 10))))
;;=> #'performance-example.performance-tips/array2
```

这等价于以下内容：

```
double[] array2;
```

让我们检查数组是否创建正确：

```
(alength ^doubles array2)
;;=> 1000
Try to get the first element.
(aget ^doubles array2 0)
;;=> 7.069889039765091
```

我们将使用 amap 和 areduce 定义 my-calc2：

```
(defn my-calc2[ar]
  (let [xs
    (amap ^doubles ar
      idx ret
      (let [x (aget ^doubles ar idx)]
        (* x x)))]
    (areduce xs i ret (double 0)
      (+ ret (aget ^doubles xs i)))))
#'performance-example.performance-tips/my-calc2
```

让我们测试两种情况的性能：

```
(bench (my-calc1 array1))
(bench (my-calc2 array2))
Using a primitive type is faster.
Execution time mean : 69.387426 µs
Execution time mean : 54.874642 µs
```

# 使用瞬态数据结构

Clojure 的 `transient` 是一种允许可变更新的数据结构。我们将学习如何使用它并将其性能与 Clojure 的持久数据结构进行比较。

下面的代码使用 `transient`：

```
(defn rand-seq1 [n m]
  (loop [i 0 s (transient [])]
    (if (< i n)
      (recur (inc i) (conj! s (rand m)))
      (persistent! s))))
```

`transient` 数据结构允许可变函数如 `conj!`、`disj!`、`pop!`、`assoc!` 和 `dissoc!`。我们不能对持久数据结构使用这些函数。要获取持久数据，请使用 `persistent!`。

我们将定义一个不使用 `transient` 的相同函数：

```
(defn rand-seq2 [n m]
  (loop [i 0 s []]
    (if (< i n)
      (recur (inc i) (conj s (rand m)))
      s)))
```

然后，让我们运行基准测试：

```
(bench (rand-seq1 1000000 10))
(bench (rand-seq2 1000000 10))
```

我们得到了以下结果，使用原始数组更快：

```
transient => Execution time mean : 86.779352 ms
non-transient => Execution time mean : 128.905613 ms
```

# 记忆化函数

我们向你介绍 memoize，它缓存函数调用的参数和结果。如果函数重复使用相同的参数调用，memoize 会提高性能；另一方面，使用 memoize 会消耗内存。

我们将在下面定义一个阶乘函数：

```
(defn factorial [num]
  (if (= num 1)
    num
    (* num (factorial (dec num))))
```

然后，我们将定义阶乘的 memoize 版本：

```
(def memoize-factorial (memoize factorial))
```

现在，让我们测量非 memoize 版本和 memoize 版本的性能：

```
(time (factorial 3000N))
;;=> "Elapsed time: 11.111595 msecs"
;;=> 4149359603......
(time (factorial 3000N))
"Elapsed time: 11.260317 msecs"
;;=> 4149359603......
```

我们运行了两次非 memoize 版本的阶乘。执行时间几乎相同。接下来，我们将测试阶乘的 memoize 版本：

```
(time (memoize-factorial 3000N))
"Elapsed time: 11.258368 msecs"
;;=> 4149359603......
(time (memoize-factorial 3000N))
"Elapsed time: 0.084887 msecs"
;;=> 4149359603......
```

第一次执行几乎与非 memoize 版本相同。但第二次调用非常快。

# 还有更多...

在这里，我们将向你展示其他用于性能分析的工具。

# 使用 timbre 记录日志并使用 tufte 进行分析

ptaoussanis/timbre 是 Clojure/Script 的日志 API。timbre 快速、灵活，并且易于使用 map 配置。ptaoussanis/tufte 是 Clojure/Clojurescript 的简单分析与性能监控工具。

为了启用 timbre 和 tufte，我们将把它们的库添加到你的 project.clj 中并重启 REPL：

```
(defproject performance-example "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :license {:name "Eclipse Public License"
            :url "http://www.eclipse.org/legal/epl-v10.html"}
  :dependencies [[org.clojure/clojure "1.8.0"]
                 [criterium "0.4.4"]
                 [com.taoensso/timbre "4.6.0"]
                 [com.taoensso/tufte "1.0.0-RC1"]])
```

然后，让我们创建 src/performance_example/timbre example.clj_。我们将添加命名空间和使用库：

```
(ns performance-example.timbre-example
  (:require
    [clojure.core.reducers :as r]
    [taoensso.timbre :as timbre
     :refer (log trace debug info warn error fatal report
             logf tracef debugf infof warnf errorf fatallf
             reportf
             spy get-env log-env)]
    [taoensso.tufte :as tufte :refer (defnp p profiled profile)]))
;;=> nil
```

现在，让我们测试日志：

```
(error "timbre error")
;;=> 16-07-18 14:40:12 phenix ERROR [performance-example.timbre-example:15] - timbre error
;;=> nil
(info "timble info")
```

```
; ;=> 16-07-18 14:40:49 phenix INFO [performance-example.timble-example:18] - timble info
; ;=> nil
```

我们测试了两个日志级别。下一个示例是使用 spy 查看表达式和结果：

```
(def x 10)
; ;=> #'performance-example.timble-example/x
(spy (* x 10) )
16-07-15 00:06:50 phenix DEBUG [performance-example.timble-example:16] - (* x 10) => 100
; ;=> nil
```

它打印表达式和求值的值。set-level! 更改当前日志级别：

```
(timbre/set-level! :info)
; ;=> {:level :info}
(debug "this is a debug message")
; ;=> nil
```

前一个示例将日志级别设置为 info。所以 debug 日志未打印。让我们将级别设置回 debug：

```
(timbre/set-level! :debug)
; ;=> {:level :debug}
(debug "this is a debug message")
; ;=> 16-07-18 13:28:03 phenix DEBUG [performance-example.timble-example:30] - this is a debug message
; ;=> nil
```

debug 日志已打印。我们将定义一个宏，用于在日志中打印表达式花费的毫秒数。我们将定义 ms-time 将纳秒转换为毫秒：

```
(defn ms-time[s]
  (double (/ (- (System/nanoTime) s) 1000000)))
; ;=> #'performance-example.timble-example/ms-time
```

然后，我们将定义一个用于在日志中打印时间的宏：

```
(defmacro with-perf-log [f s]
  `(let [st# (System/nanoTime)]
```

