

调用 Java 方法，语法如下：

```
( .method-name object arg1 arg2 ... )
```

以下代码调用 Java 的 concat 方法：

```
( .concat "Hello " "Makoto")
;;=> "Hello Makoto"
```

## 调用类方法

调用静态方法（即类方法）的语法如下：

```
(class-name/method-name arg1 arg2 ...)
```

或：

```
( . class-name method-name arg1 arg2 ... )
```

以下代码是从字符串获取浮点值：

```
(Float/valueOf "1000.0")
;;=> 1000.0
( . Float valueOf "1000.0")
;;=> 1000.0
```

另一个例子是使用静态方法获取日本时区：

```
(import 'java.util.TimeZone)
(def time-zone (TimeZone/getTimeZone "Asia/Tokyo" ))
;;=> #'chapter05.calling-java/time-zone
(.getDisplayName time-zone (java.util.Locale. "en_US"))
;;=> "Japan Standard Time"
```

## 访问实例字段

在 Java 中不推荐直接访问实例字段，因此我们很少访问实例字段。但是，有一种访问这些字段的方法。语法如下：

```
( . field-name object)
```

我们将按如下方式访问 java.awt.Point 的实例字段：

```
(import java.awt.Point)
;;=> java.awt.Point
(def p (Point. 100 200))
;;=> #'chapter05.calling-java/p
(. p x)
;;=> 100
(. p y)
;;=> 200
```

设置实例值的语法如下：

```
(set! (.field-name object) value)
```

以下代码更新 java.awt.Point 的实例字段：

```
(set! (.x p) 300)
;;=> 300
(. p x)
;;=> 300
```

## 访问类字段

访问静态字段的语法如下：

```
class-name/field-name
```

以下代码获取自然对数的值：

```
Math/E
;;=> 2.718281828459045
```

## 访问内部类

以下语法用于访问静态内部类：

```
class-name$inner-class-name/static-field-name
```

或：

```
(class-name$inner-class-name/static-method-name arg1 arg2 ..)
```

以下示例访问 Thread 类的内部类中的静态值 NEW 和 TERMINATED：

```
(def t (Thread.))
;;=> #'chapter05.calling-java/t
(.getState t)
;;=> #object[java.lang.Thread$State 0xc736e3e "NEW"]
(= (.getState t) Thread$State/NEW)
;;=> true
(.start t)
;;=> nil
(= (.getState t) Thread$State/TERMINATED)
```

下一个示例展示了如何访问内部类中的方法。创建线程时，其状态为 NEW。线程启动后立即终止，状态变为 TERMINATED：

```
(Thread$State/values)
;;=> #object["[Ljava.lang.Thread$State;" 0x3efd737c "
[Ljava.lang.Thread$State;@3efd737c"]
(Thread$State/valueOf "NEW")
;;=> #object[java.lang.Thread$State 0xc736e3e "NEW"]
```

## 引用类

要引用类，请按如下方式使用其类名：

```
String
;;=> java.lang.String
(= String (Class/forName "java.lang.String"))
;;=> true
(= java.lang.String (Class/forName "java.lang.String"))
;;=> true
```

## 使用数组

要在 Java 中创建数组，请使用 into-array：

```
(def str-array (into-array '("a" "b" "c" "d" "e")))
;;=> #'chapter05.calling-java/str-array
```

要访问数组，请使用 aget：

```
(aget str-array 0)
;;=> "a"
```

Java 中的数组不是不可变的，因此我们可以使用 aset 更改元素的值：

```
(aset str-array 0 "x")
;;=> "x"
(aget str-array 0)
;;=> "x"
```

long-array 创建元素为 Java 原始类型 long 的数组。我们将与元素为 Java long 对象的数组的性能进行比较：

```
(time
  (let [n 10000000
        a (long-array (range n))]
    (reduce + a)))
;;=> "Elapsed time: 1339.504072 msecs"
;;=> 49999995000000
(time
  (let [n 10000000
        a (into-array (range n))]
    (reduce + a)))
;;=> "Elapsed time: 6101.370939 msecs"
;;=> 49999995000000
```

以下代码创建 Java 的 byte-array：

```
(byte-array (map int [\C \l \o \j \o \u \r \e]))
;;=> #object["[B" 0x590f9b92 "[B@590f9b92"]
(String. (byte-array (map int [\C \l \o \j \o \u \r \e])))
;;=> "Clojoure"
```

以下代码用于创建二维数组：

```
(def mat1 (make-array Double/TYPE 10 10))
;;=> #'chapter05.calling-java/mat1
(aget mat1 9 9)
;;=> 0.0
```

要指定，Double/TYPE 生成元素为 double 原始类型的数组。

## 它是如何工作的...

我们将总结 Java 互操作的方式并将其语法与 Java 调用进行比较。然后我们将解释 Clojure 如何不继承 Java 的已检查异常。

## Clojure 访问 Java 方法和对象的总结

在下表中，我们总结了 Clojure 如何调用 Java 方法和访问 Java 对象：

| 操作 | 语法 | Clojure 代码 | Java 代码 |
|---|---|---|---|
| 创建实例 | (ClassName. arg1 arg2 ...) | (String. "abc") | new String("abc"); |
| 调用实例方法 | (.method-name object arg1 arg2 ...) | (.concat "Hello " "Makoto") | String s = "Hello "; s.concat("Makoto"); |
| 调用类方法 | (class-name/method-name arg1 arg2 ...) | (Float/valueOf "1000.0") | Float.valueOf("1000.0"); |
| 访问实例字段 | (. field-name object) | (. (java.awt.Point. 100 200) x) | Point new java.awt.Point(100 200); p.x; |
| 访问类字段 | class-name/field-name | Math/E | Math.E; |
| 访问内部类的字段 | class-name$inner-class-name/static-field-name | Thread$State/NEW | Thread.State.NEW; |
| 调用内部类的方法 | (class-name$inner-class-name/static-field-name arg1 arg2 ..) | (Thread$State/values) | Thread.State.values(); |
| 引用类 | class-name | java.lang.String | Class.forName("java.lang.String"); |

## Clojure 不继承 Java 的已检查异常

在 Java 中，已检查异常在编译时检查。使用 Java，已检查异常应由 try-catch 块处理或方法应使用 throws 关键字声明异常。否则程序将在编译时出错。

以下 Java 代码在编译时导致错误：

```
class CheckedException
{
    public static void main(String args[])
    {
    Thread.sleep(1000);
    }
}
```

```
$ javac CheckedException.java
CheckedException.java:3: error: unreported exception
InterruptedException; must be caught or declared to be thrown
    Thread.sleep(10000);
                ^
1 error
```

为避免前述错误，我们需要按如下方式修改代码：

```
public class CheckedException {
    public static void main(String args[])  {
    try {
        Thread.sleep(10000);
    } catch (InterruptedException e) {
        System.out.println("Interrupt occured");
    }
    }
}
```

通常，已检查异常并不受欢迎。Scala 也不继承已检查异常。Clojure 也是如此，以下代码可以正常工作：

```
(Thread/sleep 1000)
```

## 数组访问的总结

在下表中，我们总结了从 Clojure 访问 Java 的数组以及使用 Java 的等效代码：

| 操作 | Clojure | Java |
|---|---|---|
| 创建一维数组 | (long-array 10) | new long[10]; |
| 创建二维数组 | (make-array Double/TYPE 10 10) | new Double[10][10]; |
| 获取数组元素 | (aget str_array 0) | str_array[0]; |
| 设置数组元素的值 | (aset str_array 0 "x") | str_array[4] = "x"; |

## 打印 Java 原始类型的字符串

当我们生成元素类型为原始类型的数组时，我们经常会看到以下不寻常的表达式：

```
(int-array [1 2])
;;=> #object["[I" 0x4d307a58 "[I@4d307a58"]
```

在前面的示例中，“[I”代表 int 的数组。类似地，下一个示例生成具有 Double/TYPE 的二维数组：

```
(make-array Double/TYPE 10 10)
;;=> #object["[[D" 0x77570b3c "[[D@77570b3c"]
```

在前面的示例中，“[[D”代表 double 类型的二维数组。下表显示了 Java 原始类型如何打印：

| 类型 | 字符串 |
|---|---|
| Short | "S" |
| Integer | "I" |
| Long | "J" |
| Float | "F" |
| Double | "D" |
| Byte | "B" |
| Char | "C" |
| Boolean | "Z" |

## 还有更多...

我们将在本节学习如何使用更高级的主题。

## 使用双点宏进行链式调用

.. 宏展开为对第一个参数的第一个成员的成员访问 (.)，然后对结果的下一个成员。使用 (..) 从 Java 方法链式调用。获取 REPL 中 JAVA_HOME 的代码如下：

```
(.. System getenv (get "JAVA_HOME"))
;;=> "/usr/local/oss/jdk1.8.0_65"
```

前者等价于：

```
(. (. System getenv) (get "JAVA_HOME")).
(macroexpand-1 '(.. System getenv (get "JAVA_HOME")))
;;=> (..(.. (. System getenv) (get "JAVA_HOME")))
```

## 使用 doto 宏

使用 Java 互操作在同一对象上应用多个方法的代码如下：

```
(doto (java.util.HashMap.)
  (.put "key1" "value1")
  (.put "key2" "value2")
  (.put "key3" "value3")
)
;;=> {"key1" "value1", "key2" "value2", "key3" "value3"}
```

