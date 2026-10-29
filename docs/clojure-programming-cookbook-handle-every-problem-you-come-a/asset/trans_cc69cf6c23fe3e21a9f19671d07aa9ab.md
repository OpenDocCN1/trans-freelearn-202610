

它用一个例子更有意义，所以让我们来做：

```
(defmacro hello2d [arg]
  `(+ 1 ~arg))
```

我们现在引入了两个不同的字符：反引号，或 ~，以及反引号拼接，或 ~@。把反引号和反引号拼接想象成复制粘贴，你可以在生成的代码中直接插入表单。因此在前面的代码中，生成的代码应该展开为 arg 的实际值，而不是简单地是 arg 字符串：

```
(macroexpand-1 '(hello2d 3))
; (clojure.core/+ 1 3)
```

哇！这样感觉好多了，不是吗。让我们确认一下它是否能被正确求值：

```
(hello2d 3)
; 4
```

最后，反引号拼接对于列表、映射等是有意义的，但复制/粘贴的想法保持不变。一个复制并粘贴列表原样，而另一个则逐个将表单复制到模板中：

```
(defmacro hello2e [& body ]
  `(apply str ~body))
```

```
(defmacro hello2f [& body ]
  `(apply str ~@body))
```

```
(macroexpand-1 '(hello2e "1" "2" "3"))
; (clojure.core/apply clojure.core/str ("1" "2" "3"))
```

```
(macroexpand-1 '(hello2f  "1" "2" "3"))
; (clojure.core/apply clojure.core/str "1" "2" "3")
```

## 到处都是宏

在本教程中，我们看到了如何创建我们第一个宏，并继续掌握编译代码是什么以及它在代码生命周期中何时发生。我们看到 Clojure 是一种同构语言，

意味着其内部结构和其表示是相同的，并且我们将这种同构特性与宏概念联系在了一起。

## 定义更高级的宏

当我们开始思考本书的结构时，一切似乎都很有道理去谈论简单宏和高级宏。现在，到了这个主题，作者不得不承认这已经不再那么清楚了。那么，当我们谈论高级宏时，我们指的是什么呢？我们曾试图询问 Canon，但他们没有给我们一个合适的答案。

因此，在本教程中，我们将断言，高级宏是指生成一种会改变结果代码求值流程的表单的宏。在上一节关于简单宏的教程中，宏是，嗯，简单的，因为生成的表单始终相同。本教程将展示始终生成相同代码的宏，或控制流变得稍微复杂一些的宏。当然，目标是能够轻松地在你的代码中编写方便的宏。

### 准备工作

虽然本教程没有特定的准备工作，但让我们看看当我们有一个宏来指导我们时 clojure.core 的情况：

```
(defmacro when
  "Evaluates test. If logical true, evaluates body in an implicit do."
  {:added "1.0"}
  [test & body]
  (list 'if test (cons 'do body)))
```

根据我们所学的内容，我们可以用反引号字符以稍微更简洁的形式重写它：

```
(defmacro my-when
  [test & body]
  `(if ~test (do ~@body)))
```

生成的代码符合计划：

```
(macroexpand-1
  '(my-when (odd? (rand-int 2))
```

```
(println "hello")))
; (if (odd? (rand-int 2)) (do (println "hello")))
```

好。现在我们对高级宏感兴趣的是生成更高级的代码。因此，沿着这个例子，我们将编写一个随机生成代码的宏。

以下代码中的宏不再以 test 作为输入参数，而是只接受一个 body。我们只是移动了一些代码，但请看！反引号符号不再位于宏体的开头，它现在嵌套在 if 语句内部。

记住宏体需要返回一个 Clojure 表单。以下代码中的宏将随机返回传递的 body 列表包装在 do 中，或一个空列表：

```
(defmacro random-when
  [& body]
  (if (odd? (rand-int 2))
    `(do ~@body)
    (list)
    ))
```

在 REPL 中多次尝试以下操作，你会看到生成的代码并不总是相同：

```
(macroexpand-1
  '(random-when
     (println "Hello")))
; ()
; OR
; (do (println "Hello"))
```

这是一个非常愚蠢的例子，但它应该打开你的思维，让你去探索更高级宏的新思路和冒险！

### 如何做...

我们将看两个小例子，看看一些稍微高级的宏如何能促进你的开发人员生活，让你晚上喝的那杯酒更容易理解。

### 跟踪代码何时开始

第一个示例将是一个为我们生成事物的宏，`init` 被调用的时间，以及一个告诉我们系统运行了多久的函数。

该宏使用了两次对 `def` 的调用——一次用于简单值，另一次用于函数：

```
(defmacro track-init[]
  `(do
    (def init (System/currentTimeMillis))
    (def up (fn [] (- (System/currentTimeMillis) init))))
  )
```

对 Java 平台 `System` 类的调用有点令人难以承受，但基本上我们只是想访问当前时间。按如下方式展开生成的代码：

```
(macroexpand-1
  '(track-init))
```

我们看到看起来像标准的 Clojure，定义了一个值和一个函数：

```
(do
  (def my-very-first-macro/init
    (java.lang.System/currentTimeMillis))
  (def my-very-first-macro/up
    (clojure.core/fn []
      (clojure.core/-
        (java.lang.System/currentTimeMillis)
        my-very-first-macro/init))))
```

只是你没有编写这段代码，你最初唯一写的是对 `track-init` 宏的调用：

```
(track-init)
```

现在，在命名空间中调用了宏，因此在同一命名空间中我们有 `init` 和 `(up)` ：

```
init
; 1449641148783

(up)
; 596928
```

这种为你生成代码的宏在开发期间非常有帮助，所以要小心，这种宏也可能在各处留下一些 def 后的符号！

### 使用宏记录消息

在第二个示例中，我们将简单地看一个愚蠢的日志宏，当日志级别足够时给出输出消息，而如果不够则不输出。嗯。是的。我怀念那些 20 个日志框架统治其中任何一个的时代。（但那是历史。）

因此，以下定义了一个顶级日志级别，可以自动从文件中加载，但我把这留给读者。我们还定义了 log 宏，只有当日志级别设置为 DEBUG 时才输出代码：

```
(def LOG_LEVEL "INFO")

(defmacro log [msg]
  (if (= LOG_LEVEL "DEBUG")
    `(println ~msg)))
```

在这种情况下，当我们使用以下语句调用宏时：

```
(log "hello3")
; ...
```

你猜对了。控制台中什么也没有打印。现在，假设我们再次 def 日志级别，（嗯……这不是推荐的做法，但请在这里忍耐一下我们）：

```
(def LOG_LEVEL "DEBUG")
```

再次调用 log 宏时，消息会在控制台中显示：

```
(log "hello3")
; .. hello3 in the console
```

我们将让读者像往常一样使用 macroexpand-1 检查每种情况下生成的代码：

```
(macroexpand-1
  ' (log "hello3"))
; (println "hello3")
```

### 它是如何工作的...

我想我们已经看到了很多关于在玩宏时底层过程是什么的内容。让我们看一下在互联网上随处可见的宏之一，用于创建一个看起来像循环的迭代语言。

loop 宏的代码只比你到目前为止看到的稍微困难一点。注意宏的返回表单是一个列表，通过反引号，并且 body 步骤被包装在 do 中：

```
(defmacro my-for-loop [ [sym init check change] & steps]
  `(loop [~sym ~init value# nil]
     (if ~check
       (let [new-value# (do ~@steps)]
         (recur ~change new-value#))
       value#)))
```

你可能注意到的一个新东西是这里和那里的 # 符号，例如 new-value#。# 符号表示你希望为变量或函数获取一个生成的、限定名称：

```
gensym is the longer version of this #.
(gensym "hello")
; hello6075
```

如果我们不使用 gensym 或 #，我们将尝试获取 new-value 的值，该值在宏代码生成时未定义，因此会抛出错误。

要生成自己的循环，你将按定义调用宏，使用四个参数和一个主体：

```
(my-for-loop
  [ i 0 (< i 10) (inc i) ]
```

