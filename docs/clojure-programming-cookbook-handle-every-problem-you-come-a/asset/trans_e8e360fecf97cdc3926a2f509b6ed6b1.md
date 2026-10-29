

bigdec 函数将任意数字转换为 BigDecimal。后缀 M 表示 BigDecimal。虽然它可以表示非常大的数字，但它无法表示循环小数并会引发错误：

```
(bigdec (/ 10 2))
;;=> 5M
(bigdec (/ 10 3))
;; ArithmeticException Non-terminating decimal expansion; no exact representable decimal result. ;;
java.math.BigDecimal.divide (BigDecimal.java:1690)
```

Clojure 还提供大整数 clojure.lang.BigInt。它等价于 Java 的 java.math.BigInteger：

```
(= (bigint 10) 10N)
;;=> true
```

## 使用字符串和字符

Clojure 字符串是 Java 的 String(java.lang.String)，用双引号 (") 括起来：

```
"Hello world ! "
;;=> "Hello world ! "
```

Clojure 与 Java 具有良好的互操作性，很容易从 Clojure 调用 Java 的成员方法。在以下示例中，代码调用了 Java String 类中的 concat 方法：

```
(.concat "Hello " "world !")
;;=> "Hello world !"
```

以下语法是从 Clojure 调用 Java 成员方法的方式：

```
(.method-name object arg1 arg2 ...)
```

第一个参数是点 (.) 前缀后跟方法名，然后是对象及其参数。

等价的 Java 代码如下：

```
String str = "Hello ".concat("world !");
```

str 函数类似于 concat 方法，但它可以接受任意数量的参数：

```
(str "Hello " "world !" " Clojure")
;;=> "Hello world ! Clojure"
```

要检查字符串的长度，请使用 length 方法或 count 函数：

```
(.length "Hello world !")
;;=> 13
```

clojure.string 是用于操作字符串的内置 Clojure 库。这里，我们展示 clojure.string 中的一些函数：

```
(clojure.string/blank? "   ")
;;=> true
(clojure.string/trim "  Hello ")
;;=> "Hello"
(clojure.string/upper-case "clojure")
;;=> "CLOJURE"
(clojure.string/capitalize "clojure")
;;=> "Clojure"
(clojure.string/lower-case "REPL")
;;=> "repl"
```

让我们进入 Clojure 字符类型。Clojure 中的字符是 java.lang.Character，以反斜杠 (\) 为前缀：

```
\a
;;=> \a
```

int 函数获取字符的整数值。同时，char 从字符获取整数值：

```
(int \a)
;;=> 97
(char 97)
;;=> \a
```

seq 函数从字符串返回字符序列，str 从字符生成字符串：

```
(seq "Hello world!")
;;=> (\H \e \l \l \o \space \w \o \r \l \d \!)
(str \C \l \o \j \u \r \e)
;;=> "Clojure"
```

## 使用布尔值和 nil

Clojure 的布尔值是 java.lang.Boolean，其值是 true 和 false：

```
true
;;=> true
false
;;=> false
(= 1 1)
;;=> true
(= 1 2)
;;=> false
(= "Hello" "Hello")
;;=> true
(= "Hello" "hello")
;;=> false
```

not 函数如果表达式在逻辑上为假则返回 true。否则返回 false：

```
(not true)
;;=> false
(not false)
;;=> true
(not= 1 2)
;;=> true
(not true)
;;=> false
```

true? 函数如果表达式为 true 则返回 true；否则返回 false。false? 函数如果表达式为 true 则返回 false；否则返回 true：

```
(true? (= 1 1))
;;=> true
(false? (= 1 1))
;;=> false
```

nil 函数表示无，或值的缺失。Clojure 中的 nil 函数几乎等价于 Java 中的 null 函数。nil 在 Clojure 条件中在逻辑上为假。nil 通常用作返回值以表示假：

```
(if nil true false)
;;=> false
```

nil 是用于表示空序列的返回值：

```
(seq [])
;;=> nil
```

当映射集合查找时找不到给定键，默认返回 nil：

```
(get {:a 1 :b 2} :c)
;;=> nil
```

## 使用符号和关键字

def 函数将全局符号绑定到其值。下一个示例显示 def 将 pi 符号绑定到值 (3.14159265359)：

```
(def pi 3.14159265359)
;;=> #'user/pi
pi
;;=> 3.14159265359
pi-not-bind
;;=> CompilerException java.lang.RuntimeException: Unable to resolve symbol:
;;     pi-not-bind in this context, compiling: (/tmp/form-init1426260352520034213.clj:1:7266)
```

关键字与符号类似，但关键字求值时返回自身：

```
:key1
;;=> :key1
```

关键字用于标识事物，因此它们经常在 Clojure 的映射中使用：

```
(def person {:name "John McCarthy" :country "USA"})
;;=>#'living-clojure.core/person
```

## 它是如何工作的...

Clojure 原始数据类型大多是 Java 类，但某些数据类型是 Clojure 自有类。

对于数字，Clojure 的整数是 `java.lang.Long`，浮点数是 `java.lang.Double`。`bigInt` 和比率分别是 `clojure.lang.BigInt` 和 `clojure.lang.Ratio`：

```
(class 1)
;;=> java.lang.Long
(class 1.0)
;;=> java.lang.Double
(class (float 1.0))
;;=> java.lang.Float
(class 5.5M)
;;=> java.math.BigDecimal
(class 1N)
;;=> clojure.lang.BigInt
(class (/ 10 3))
;;=> clojure.lang.Ratio
```

Clojure 的字符串和字符分别是 `java.lang.String` 和 `java.lang.Character`：

```
(class "Hello world ! ")
;;=> java.lang.String
(class \a)
;;=> java.lang.Character
```

Clojure 的布尔值是 `java.lang.Boolean`：

```
(class true)
;;=> java.lang.Boolean
(class false)
;;=> java.lang.Boolean
```

Clojure 的关键字和符号是 Clojure 自有类：

```
(class :key)
;;=> clojure.lang.Keyword
(class (quote a))
;;=> clojure.lang.Symbol
(class nil)
;;=> nil
```

## 还有更多...

Clojure 支持十六进制和八进制表示法：

```
0xff
;;=> 255
0400
;;=> 256
```

Clojure 还支持灵活的数字基数。你可以用 2 到 36 的基数指定任意基数：

```
2r11111
;;=> 31
16rff
;;=> 255
8r11000
;;=> 4608
7r111
;;=>57
```

有一些特别命名的字符字面量，如下所示：

- \space
- \tab
- \newline
- \return
- \formfeed
- \backspace

Unicode 字符在 Clojure 中由 \uNNNN 表示，如 Java 一样：

```
\u0031\u0032\u0061\u0062
;;=> \1
;;=> \2
;;=> \a
;;=> \b
```

Clojure 中的函数也是符号并绑定到其函数定义。下一个示例显示 + 的值：

```
+
;;=> #object[clojure.core$_PLUS_ 0x3c2ace8a "clojure.core$_PLUS_@3c2ace8a"]
```

要求值符号并返回自身，请使用 quote：

```
(quote pi-not-bind)
;;=> pi-not-bind
```

要在 Clojure 中使用数学库，请尝试 clojure.math.numeric-tower。以下是步骤：

- 1. 将以下依赖项添加到 project.clj 中：
  [org.clojure/math.numeric-tower "0.0.4"]
- 2. 你可以在库中使用 expt 和 sqrt：
  (require '[clojure.math.numeric-tower :as math])
  (math/expt 2 10)
  ;;=> 1024
  (math/sqrt 10)
  ;;=> 3.1622776601683795

## 使用 vars、条件、循环和错误处理的绑定

在本教程中，我们将回顾与 vars 和值、条件、迭代和循环相关的 Clojure 编程控制结构。我们将使用以下特殊形式、宏和函数：

- `def` 和 `let`
- `if` 和 `if-not`
- `when` 和 `when-not`
- `case` 和 `cond`
- `do` 和 `dotimes`
- `loop` 和 `recur`
- `try... catch... throw`

## 准备工作

你只需要 REPL，如本章第一个教程所述，且不需要额外库。启动 REPL，以便你可以在本教程中立即测试示例代码。

## 如何操作...

让我们从如何使用 `def` 和 `let` 绑定 vars 开始。

### def 和 let

`def` 是一个特殊形式，在其命名空间中将符号绑定到全局作用域。`def` 需要 var 和值：

```
(def var val)
```

此示例将 x 绑定到 100：

```
(def x 100)
;;=> 100
```

而 let 在其局部作用域中绑定符号。你可以在 let 子句中放置多个表达式。let 评估它们并返回最后一个表达式：

```
(let [var-1 val-1 var-2 val-2 ...]
    expr-1
    expr-2
    ....
)
```

在此示例中，let 将 x 绑定到 3，y 绑定到 2。然后，它连续评估两个表达式并返回第二个表达式：

```
(let [x 3 y 2]
    (println "x = " x ", y = " y)
    (* x y)
)
;;=> x =  3 , y =  2
;;=> 6
```

### if 和 if-not

if 接受三个参数；第三个参数（else-expression）是可选的：

```
(if condition then-expression else-expression)
```

在下一个示例中，代码返回数字的绝对值：

```
(let [x 10]
    (if (> x 0) x (- x))
)
;;=> 10
(let [x -10]
    (if (> x 0) x (- x))
)
;;=> 10
```

如果没有第三个参数且测试结果为假，则返回 nil：

```
(let [x -10]
    (if (> x 0) x))
;;=> nil

