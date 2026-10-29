

# 使用 Immutant - 重用基础设施

Immutant 并不是你常见的 Clojure Web 框架。Immutant 允许你构建到现有的、健壮的中间件 WildFly，也就是 JBoss 中，并以优雅的方式部署你的软件。在本教程中，你将看到如何通过 Immutant 框架提供的各种服务来增强你的 ring 应用。

虽然 Immutant 使部署到 WildFly 变得简单，但开发环境完全自包含，可以在无需额外下载或安装的情况下完成。让我们来看看吧！

## 准备工作

本节的第一部分可能完全令人失望；在此时，我们完全处于 Clojure 的领域，一切开箱即用。糟糕，对吧？

以下是 project.clj 文件，其中列出了所需库的完整列表：

```
(defproject mutant1 "0.1.0-SNAPSHOT"
  :dependencies [
    [org.clojure/clojure "1.8.0"]
    [org.immutant/immutant "2.1.4"]
    [ring/ring-devel "1.5.0"]])
```

添加 ring-devel 是为了允许开发模式和重新加载工作流中的刷新。

构建 Web 应用的代码遵循 ring 路由模式，因此以下内容大部分是标准的：

```
(ns mutant1.core
  (:require [immutant.web :as web]))

(defn app [request]
  {:status 200
:body "Hello world"})

(defn -main []
(web/run-dmc app))
```

`web/run-dmc` 方法用于开发模式，而 `web/run` 将用于生产环境。

在命令行中，标准的：`lein run -m mutant1.core` 就足够让你开始了，并且可以看到美丽的页面呈现在你面前：

![](img/d7b631352a782ee50160cf8eb995152f_555_0.png)

## 如何操作...

我们已经看到了基础知识，以及如何准备；现在让我们看看在使用 Immutant 时如何与不同的中间件框架交互。

### 使用 Artemis 进行简单消息传递

本示例将展示如何启动一个消息服务器，并创建消息上下文和队列，以及通过 Clojure 代码发送和监听该队列。

这里展示的内容实际上是相当非官方的，并不是因为它质量为 alpha，而是因为使事情变得容易的一些部分和命名空间最近发生了变化。

因此，与其使用在 Interweb 上大量文档中提到的 `hornetq` 框架，我们将使用 WildFly 最新版本中包含的新库消息框架 Artemis。

由于我们希望避免包含 hornetq 库，project.clj 文件变得稍微复杂一些，但这很快就会被修复：

```
(defproject mutant2 "0.1.0-SNAPSHOT"
  :dependencies [
    [org.clojure/clojure "1.8.0"]
    [org.immutant/immutant "2.1.4"
      :exclusions [org.projectodd.wunderboss/wunderboss-messaging-hornetq]]
    [org.projectodd.wunderboss/wunderboss-artemis "0.2.0"]
    [ring/ring-devel "1.5.0"]
    [compojure "1.5.0"]])
```

你也可以注意到 Compojure 的（重新）引入，并且你就可以开始了！

以下片段实际上要求框架通过略显误导的 msg/context 函数创建一个新的消息服务器：

```
(require '[immutant.web :as web] '[immutant.messaging :as msg])
(def messaging-server
  (msg/context :remote-type :hornetq-standalone))
```

一旦消息服务器以编程方式启动，我们就可以创建一个上下文，用于查找队列和主题。

在这里，我们连接到刚刚启动的消息服务器，并查找 hello 队列。这实际上会在队列不存在时创建队列，或者如果另一个服务已经注册了它，则简单地连接到它：

```
(def mycontext
  (msg/context :host "localhost"))

(def queue
  (msg/queue "hello" :context mycontext))
```

大多数新的函数调用都是使用 immutant.messaging 命名空间完成的，标记为 msg。

现在你有了队列，是时候监听它并发送消息了。listen 函数接受一个队列或主题以及一个回调，作为一个简单的 Clojure 函数：

```
(msg/listen queue
  (fn[msg]
    (println msg)))
```

然后一些常规的 Compojure 路由将允许你向同一队列发送消息：

```
(require '[compojure.core :refer :all]
        '[compojure.route :as route])
(defroutes app
  (GET "/" [] "<h1>Hello Compojure World</h1><br/><a href=\"/msg\">Send a message</a>")
  (GET "/msg" [] (msg/publish queue "world") "OK")
  (route/not-found "<h1>Page not found</h1>"))
```

开始吧：

```
(defn -main []
  (web/run-dmc app :host "0.0.0.0" :port 8080))
```

现在我们可以打开服务器在 8080 端口提供的页面：

![](img/d7b631352a782ee50160cf8eb995152f_557_0.png)

无论如何，在你的应用程序中你现在可以在哪里使用这种消息机制？通过注册到来自消息系统的消息。

如果一个消息系统只能与自身对话，那就不算是一个合适的消息系统了，所以让我们看看如何设置远程连接！

### 远程消息传递

现在我们希望设置两个服务器使用 WildFly 消息队列相互通信。

第一个服务器的设置几乎与前一个示例相同，只是为了远程连接，我们需要绑定到一个不同的、可外部访问的 IP 地址。这是通过向 Leiningen JVM 选项添加 JVM 设置来完成的，因此在 project.clj 中，我们将添加以下内容：

```
:jvm-opts ["-Dactivemq.netty.host=0.0.0.0"]
```

如果你仔细查看控制台日志，你会注意到消息系统确实使用上述设置绑定：

```
22:46:55.000 INFO [org.apache.activemq.artemis.core.server]
(main) AMQ221020: Started Acceptor at 0.0.0.0:5445 for
protocols [CORE]
22:46:55.003 INFO [org.apache.activemq.artemis.core.server]
(main) AMQ221007: Server is now live
```

太棒了。现在来设置第二个服务器。在本书的上下文中，第二个服务器实际上运行在 Raspberry Pi 上，最困难的事情实际上是让 IP 地址按正确的顺序排列。

这个第二个服务器使用与你到目前为止看到的类似的 Clojure 代码。队列查找和 ring 处理程序路由基本上是相同的。变化的是从远程位置检索队列的方式。

第一个服务器的 IP 为 192.168.1.3，因此以下示例将使用该地址。我们还给出了最多 30 次重连次数，以便如果第一个服务器宕机，这仍会等待并尝试获取连接。（试试看！！）

出发吧：

```
(def mycontext
  (msg/context
    :host "192.168.1.3" :port 5445 :reconnect-attempts 30))

(def queue
  (msg/queue "hello" :context mycontext))

(defroutes app
(GET "/" [] (msg/publish queue "world") "OK"))
```

第二个服务器以相同的命令启动：

```
lein run -m mutant3.core
```

这里的 Raspberry Pi 的 IP 地址是 192.168.1.7，所以让我们连接到它：

![](img/d7b631352a782ee50160cf8eb995152f_559_0.png)

对于一个 Web 应用来说，这并不太健谈，但第一个服务器会相应地收到一条爱的消息，并且 hello world 会显示在控制台日志中。耶！远程通信。

### 使用队列进行远程过程调用

扩展前一个示例，Immutant 还提供了一种通过排队系统轻松进行 **远程过程调用 (RPCs)** 的方法。

这可以成为将负载分发到不同的 Raspberry Pis 的好方法！

让我们通过这个 RPC 框架来完成一个分布式 hello world。

第一个设置需要像上一教程中那样设置。我们将使用一个新的队列，`queue2`（富有创造性的命名是当今的潮流……）。这个 `queue2` 然后将从 respond 处理程序中使用，这是过程调用的入口，如下所示：

```
(def queue2 (msg/queue "hello2" :context mycontext))
```

```
(msg/respond
  queue2
  (fn[msg] (str "hello " msg)))
```

在这里，处理程序接收一个参数，并发送一些礼貌的问候消息。

请注意，每个 respond 处理程序都需要一个专用的队列。

在第二个服务器上，我们将查找 `queue2`，并创建一个 Compojure 路由来发送 RPC 请求。`msg/request` 函数调用实际上返回一个 future，因此如果我们想要同步答案，我们将使用 @ 符号等待执行结果。

这转化为：

```
(def queue2
  (msg/queue "hello2" :context mycontext))
```

```
(defroutes app
  (GET "/req" [] @(msg/request queue2 "world")))
```

这个第二个服务器再次运行在 Raspberry Pi 上。对它的请求会将消息发布到队列，该消息将被第一个服务器拾取，回调将被执行，结果发送回第二个服务器：

