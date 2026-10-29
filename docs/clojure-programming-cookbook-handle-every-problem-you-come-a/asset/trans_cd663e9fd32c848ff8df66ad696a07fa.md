

```
(pmap
#(async/>!! in-chan %)
(line-seq in)))
```

假设 test_0.xml 文件内容如下：

```
<friend age="23">Friend1</friend>
<friend age="23">Friend4</friend>
...
```

那么使用 core-async 读取的输出就简单地按一个接一个的流进行，如下图所示：

![](img/d7b631352a782ee50160cf8eb995152f_284_0.png)

## 还有更多...

相同的数据解析和处理技术也可以应用于 reducers：

```
(require '[clojure.core.reducers :as r])
(use 'clojure.data.xml)
(use '[clojure.java.io :only [writer reader input-stream output-stream]])
```

```
(with-open [in (reader (input-stream "test_1.xml"))]
(into []
(r/filter
```

```
(fn[x]
  (= "Friend1"
  (first (:content (parse
  (java.io.StringReader. x))))))
(line-seq in)))
```

这有在内存中加载比所需更多数据的缺点，但处理起来更容易阅读且简洁。

如果在处理数据时要执行多个操作，还可以加入 transducers。我们把这留给读者的好奇心！

## 使用 Apache Camel 连接一切

Apache Camel 是一个基于规则的路由和中介引擎，它使用 API 或声明式 Java 领域特定语言来配置路由和中介规则，提供企业集成模式。

## 准备工作

在本配方中，我们将使用 Java Camel 库。因此，你需要在 project.clj 中添加 org.apache.camel/camel-core：

```
(defproject chapter04 "0.1.0"
:dependencies [[org.clojure/clojure "1.8.0"]
                [org.apache.camel/camel-core "2.16.1"]]
```

## 如何操作...

在这里，我们将学习从 Clojure 使用 Camel 的基本用法。

## 导入 Camel 类

以下代码导入我们将使用的 Camel 类：

```
(ns chapter04.camel
  (:import [org.apache.camel.impl DefaultCamelContext]
           [org.apache.camel.builder RouteBuilder]
           [org.apache.camel.main Main]
           [org.apache.camel Exchange]
           [org.apache.camel Processor]))
```

## 使用 Clojure 搭乘你的第一次 Camel 之旅

让我们编写一段简单的 Camel 代码，将文件从一个目录移动到另一个目录。以下代码将文件从 /tmp/in 移动到 /tmp/out：

```
(def ctx
```

```
(let [context (DefaultCamelContext.)]
  (.addRoutes context (proxy [RouteBuilder] []
    (configure [] (.. this
      (from
"file:/tmp/in?noop=false")
      (to
"file:/tmp/out")))))
  context
  ))
#'chapter04.camel/ctx
```

启动上下文：

```
(.start ctx)
;;=> nil
```

然后在 /tmp/in 下创建一个文件：

```
$ echo hello Makoto ! > /tmp/in/makoto.txt
```

Camel 代码检查 /tmp/in 并移动到 /tmp/out：

![](img/d7b631352a782ee50160cf8eb995152f_287_0.png)

让我们按如下方式停止 Camel：

```
(.stop ctx)
;;=> nil
```

## 使用 Camel 编写更符合 Clojure 习惯的代码

与以下 Java 代码相比，前面的代码更简单：

```
public class FileToFile {
    public static void main(String args[]) throws Exception {
        CamelContext context = new DefaultCamelContext();
```

```
context.addRoutes(new RouteBuilder() {
    public void configure() {
        from("file:/tmp/in?noop=false")
            .to("file:/tmp/out");
    }
});
context.start();
}
```

使用宏可以使应用程序代码更简单。我们将定义一个实现 org.apache.camel.builder RouteBuilder 的辅助函数和一个宏：

```
(defn create-builder [builder]
  (proxy [RouteBuilder] [] (configure [] (builder this))))
;;=> #'chapter04.camel/create-builder
(defmacro add-routes [ctx & body]
  `(.addRoutes ~ctx (create-builder (fn [~'this] (.. ~'this ~@body)))))
;;=>#'chapter04.camel/add-routes
```

使用宏，代码要简单得多，路由定义也易于理解：

```
(defn file-to-file []
  (let [context (DefaultCamelContext.)]
    (add-routes context
      (from "file:/tmp/in?noop=false")
      (to "file:/tmp/out")
      )
    context
  ))
;;=> #'chapter04.camel/file-to-file
```

我们可以使用 .start 和 .stop 分别启动和停止路由器：

```
(def ctx (file-to-file))
;;=> #'chapter04.camel/ctx
(.start ctx)
;;=> nil
(.stop ctx)
;;=> nil
```

## 它是如何工作的...

前面的代码使用了文件端点。我们将展示端点的主要参数如下：

| 名称 | 默认值 | 描述 |
|---|---|---|
| noop | false | 如果为 true，则文件不会以任何方式移动或删除。此选项适用于只读数据或 ETL 类型需求。 |
| append | true | 写入时，我们是追加到文件末尾还是替换它？ |
| consumer.delay | 500 | 下一次轮询文件/目录前的毫秒数。 |

## 还有更多...

让我们定义一个非常简单的处理器，在文件端点之间转换消息。要编写处理器，我们必须在 org.apache.camel Processor 类中实现 process 方法。我们将使用 Clojure proxy 函数来实现此方法。我们的简单处理器回显入站字符串，并在其上添加字符串 "Hello " 并发送到出站：

```
(defn get-payload-as-string [ex]
  (-> ex .getIn (.getBody (class ""))))
;;=> #'chapter04.camel/get-payload-as-string
(def simple-processor
  (proxy [Processor] []
    (process [exchange]
      (let [payload
            (get-payload-as-string exchange)]
        (println payload)
        (-> exchange .getOut (.setBody (str "Hello " payload)))))))
#'chapter04.camel/processor
```

因此，让我们使用处理器定义一个新的上下文：

```
(defn file-to-file-with-processor []
```

```
(let [context (DefaultCamelContext.)]
  (add-routes context
    (from "file:/tmp/in?noop=false")
    (process simple-processor)
    (to "file:/tmp/out"))
  context))
;;=> #'chapter04.camel/processor
```

以下代码运行 Camel：

```
(def ctx (file-to-file-with-processor))
;;=> #'chapter04.camel/ctx
(.start ctx)
;;=> nil
```

让我们运行以下命令：

```
$ echo Makoto > /tmp/in/makoto.txt
$ echo Nico > /tmp/in/nico.txt
```

回显字符串显示在你的控制台上，文件内容被转换，如以下屏幕截图所示：

![](img/d7b631352a782ee50160cf8eb995152f_290_0.png)

## 另请参阅

你可以在 http://camel.apache.org/ 查看 Apache Camel 主页。

# 第 5 章. 与其他语言协作

在本章中，我们将涵盖以下主题：

- 从 Clojure 调用 Java 方法和访问 Java 对象
- 扩展 Java 超类和实现 Java 接口
- 从 Java 调用 Clojure
- 从 Clojure 调用 Scala
- ClojureCLR
- ClojureScript

## 简介

Clojure 运行在 **Java 虚拟机 (JVM)** 上，Clojure 语言提供对 Java 的完全访问。Java 拥有庞大的资产，我们可以从 Clojure 使用它们全部。这也是 Clojure 的一大优势，它也可以从 Java 调用。

在本章中，你将看到如何从类实例化对象以及如何访问 Java 对象，包括方法和字段，并学习继承超类和实现接口的方法。

你还将学习如何使用 ClojureCLR 和 ClojureScript 在 Microsoft 的 .Net 和 JavaScript 上使用 Clojure。本章将向你介绍如何设置 ClojureCLR 和 ClojureScript 环境以及如何在它们上顺畅地编写代码。

## 从 Clojure 调用 Java 方法和访问 Java 对象

尽管 Clojure 语言看起来与 Java 非常不同，但 Clojure 提供了对 Java 的完全访问。这使得 Clojure 能够非常高效地利用庞大的 Java 资产。一般来说，从 Clojure 调用 Java 资产比从 Java 调用它们要简单得多，代码也更少。另一个巨大优势是 REPL，它使编程变得有趣且更快。

## 准备工作

在本配方中，我们不使用外部库，因此唯一必要的事情是启动 REPL 来运行代码。

## 如何操作...

我们将学习如何实例化对象、调用实例和类方法、访问字段和内部类以及引用类。然后，我们将学习如何操作 Java 数组。

## 实例化对象

要从类实例化对象，使用以下语法：

```
(ClassName. arg1 arg2 ...)
```

例如，要实例化一个新的 String 对象，语法如下：

```
(String. "abc")
;;=> "abc"
(Integer. 1)
;;=> 1
```

## 调用实例方法

