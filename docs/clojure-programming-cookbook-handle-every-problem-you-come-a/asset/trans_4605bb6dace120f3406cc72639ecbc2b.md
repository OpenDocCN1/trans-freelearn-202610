

```
(:import [System.Text])
(:import [System.Text Encoding]))
```

在纯 Clojure 中，我们将创建一个函数，该函数将从字符串拼接中创建 API 端点。这在 Yahoo API 用法中大多有记录，并且仅提取了城市和国家以增加一点乐趣：

```
(defn api-uri [city country]
  (str "https://query.yahooapis.com/v1/public/yql?
q=select%20astronomy.sunset%20from%20weather.forecast%20where%20woeid%20in%20(select%20woeid%20from%20geo.places(1)%20where%20text%3D%22"
    city
    "%2C%20"
    country
"%22)&format=xml&env=store%3A%2F%2Fdatatables.org%2Falltableswithkeys"))
```

然后可以按如下方式调用前面的函数：

```
(api-uri "Tokyo" "Japan")
; ... a long string ...
```

让我们将 uri ref 定义为最后一次函数调用的返回值；请记住在 REPL 中引用之前结果的 * 标记：

```
(def uri *1)
```

然后让我们使用该 URI 端点通过 WebClient 查询日落时间：

```
(.GetString Encoding/UTF8 (.DownloadData (WebClient.) uri))
; "<?xml version="1.0" encoding="UTF-8"?>\n<query
xmlns:yahoo="http://www.yahooapis.com/v1/base.rng"
yahoo:count="1" yahoo:created="2016-02-09T02:04:09Z"
yahoo:lang="en-US"><results><channel><yweather:astronomy
xmlns:yweather="http://xml.weather.yahoo.com/ns/rss/1.0"
sunset="5:15 pm"/></channel></results></query><!-- total: 9 -->
<! -- main-37493228-cac5-11e5-b8c6-9cb6548278d0 -->\n"
;
```

只要你没有在代理后面并且已连接到互联网，前面的调用将返回 Yahoo Web 服务的 XML 响应。结合来自前一个示例的解析调用，我们现在可以创建一个函数，该函数将查询 Web 服务并仅返回标签的属性 sunset 的我们想要的值：

```
(defn get-sunset [city country]
  (let [
    uri (api-uri city country)
    res (.GetString Encoding/UTF8 (.DownloadData (WebClient.) uri))
    reader (XmlReader/Create (StringReader. res))
  ]
  (doto reader
    (.ReadToFollowing "astronomy" "http://xml.weather.yahoo.com/ns/rss/1.0")
    (.MoveToAttribute "sunset"))
  (str (.Value reader))))
```

这很不错，我们现在有了一种方法可以查询世界各地的日落时间。所以让我们稍后前往巴黎吧！

```
(get-sunset "Paris" "France")
; 6:00pm, at the time of the book writing ;
```

## UI 原型设计

现在为了将这一切收尾，我们将额外迈出一步，为我们调用 Yahoo API 的精彩函数设置一个基本用户界面（UI）。首先，我们将构建一个非常简单的 UI，然后我们将增强我们的示例以基于 ComboBox 调用 API！兴奋吗？当然你会！

我们首先加载并引用通常的一组类：

```
(System.Reflection.Assembly/LoadWithPartialName "System.Windows.Forms")

(ns hello.simpleui
  (:import [System.Windows.Forms Button Form Label])
  (:import [System.Drawing Size Point]))
```

然后，我们使用 let 块定义整体 UI 的新组件：

```
(let [
  form (Form.)
  load-btn (Button.)
```

```
title-label (Label.)
title-string "A very simple UI! "]
```

我们然后创建一个标准标签；注意互操作和对设置器的调用是使用 .set_ 调用对不同字段完成的：

```
(doto title-label
  (.set_Text title-string)
  (.set_Location (Point. 12 12))
  (.set_Size (Size. 360 22)))
```

要构建一个交互式按钮，我们使用 gen-delegate，这使我们能够插入到原生的事件处理系统中。gen-delegate 的块是当事件被触发时将被调用的回调的主体。在这里我们更改前一个标签的文本并在控制台上打印一个字符串：

```
(doto load-btn
  (.set_Name "loadButton")
  (.set_Location (Point. 12 78))
  (.set_Text "Button 1")
  (.set_Size (Size. 360 22))
  (.add_Click
    (gen-delegate EventHandler [sender args]
      (.set_Text title-label "clicked button1 !")
      (System.Console/WriteLine "Now we use Console Writeline"))))
```

我们通过将前两个 UI 控件添加到表单中然后显示表单本身来完成所有这些操作：

```
(doto (.Controls form)
  (.Add title-label)
  (.Add load-btn))
```

```
(doto form
  (.set_Text title-string)
  (.set_Size (Size. 400 200))
  .ShowDialog)
```

![](img/d7b631352a782ee50160cf8eb995152f_345_0.png)

## 将其全部整合在一起

通过将顶级命名空间更改为包含你为 Yahoo API 编写的代码，你现在将添加一个 ComboBox 对象以能够选择城市并在 ComboBox 选择更新时发送请求：

```
(ns hello.simpleui2
  (:use [hello.net])
  (:import [System.Windows.Forms ComboBox MessageBox Button Form Label])
  (:import [System.Drawing Size Point]))
```

现在进入 ComboBox 核心代码；导入部分在 add_SelectedIndexChanged 调用上。事件本身是 SelectedIndexChanged，并且 CLR 与 Clojure 的互操作允许你通过带前缀的函数 add_ 来注册处理程序，如下所示：

```
( .AddRange
  (.get_Items combo)
  (into-array ["osaka" "Tokyo" "Sapporo"]))
```

```
(doto combo
  (.set_Location (Point. 12 78))
  (.set_Size (Size. 360 22))
  (.add_SelectedIndexChanged
    (gen-delegate EventHandler [sender args]
      (let [city (str (.Text combo))]
        (MessageBox/Show (str "sunset time in " city " : "
        (hello.net/get-sunset city "jp")))))))
```

其余代码很容易跟随，你当然会注意到对 get-sunset 函数的调用，你之前编写过。通过 REPL 或通过命令 lein clr run -m hello.simpleui2 运行所有这些，你现在将能够获取那些美丽的日本城市的日落时间：

![](img/d7b631352a782ee50160cf8eb995152f_346_0.png)

## 它是如何工作的...

现在你已经看到了大量 CLR 互操作的列表，是时候看看当从 .NET 调用 Clojure 代码时所有这些是如何工作的了。事情变得有点复杂，所以你必须保持专注。首先，你需要能够从你编写的 Clojure 代码中生成可执行文件和动态库。一旦所有这些顺利进行，我们将切换到将生成的对象导入到 Visual Studio 项目中，并且为了完整性，也导入到 Xamarin Mono-based 项目中。

## 为 .NET 编译 Clojure 代码

回到你之前创建的 hello.net 命名空间，通过添加通常的互操作胶水；我们可以添加一个 main 方法，当调用可执行文件时它将成为主要入口点：

```
(defn -main [& args]
  (println (get-sunset (first args) "jp")))
```

我们还确保命名空间定义中存在 :gen-class 指令：

```
(ns hello.net
  (:gen-class))
```

从命令行，预计结果是，接收城市作为参数，运行时将调用 Yahoo API。让我们用 Leiningen CLR 插件的 run 命令来检查一下：

```
lein clr run -m hello.net "Osaka"
; 5:34 pm
```

你很好。（你是对的？）现在为了使其可重用，我们将使用 Leiningen CLR 插件的 compile 命令来编译一个命名空间：

```
lein clr compile hello.net
```

请注意，这也会编译所需的命名空间。如果一切顺利，在 target\clr\bin 文件夹中，你现在有更多文件，尤其是两个新生成的 - DLL 和 EXE 文件：

```
-a----      2016/02/09      17:21          10240
hello.net.clj.dll
-a----      2016/02/09      17:21          4608 hello.net.exe
```

这些文件是可执行文件，前提是你已按前面解释添加了所需的 Clojure 和 Microsoft DLL。在命令行中，这次不需要插件！这是纯 .NET 代码：

```
.\hello.net.exe Osaka
; 5:34 pm
```

甜的！

为了更准备好下一步，我们将添加一个特定的方法，通过 DLL 生成并暴露。这是在 :gen-class 指令级别完成的：

```
(:gen-class
    :methods [[#^{:static true} getsunset [String] String]])
```

这告诉编译器暴露一个名为 getsunset 的函数，该函数以字符串作为输入参数，在括号中，并返回一个字符串，最右边的字符串定义。为此工作，我们实际上需要在 hello.net 命名空间的 Clojure 代码中添加一个新方法，因为暴露的方法定义默认需要使用 - 前缀：

```
(defn -getsunset [this city]
  (get-sunset city "jp"))
```

让我们重新运行 Leiningen CLR 插件，以确保你拥有最新的代码，使用：

```
lein clr compile hello.net
```

还在吗？太好了！让我们进行更多 C# 冒险吧！

## 向 Visual Studio 项目添加 Clojure 代码

所以，我们现在回到 Visual Studio，让我们创建一个全新的基于 C# 的命令行应用程序。在新创建的项目上，确保你首先通过 NuGet 添加 Clojure 库，如你之前所见，然后添加对我们新编译的二进制文件的引用：

![](img/d7b631352a782ee50160cf8eb995152f_348_0.png)

