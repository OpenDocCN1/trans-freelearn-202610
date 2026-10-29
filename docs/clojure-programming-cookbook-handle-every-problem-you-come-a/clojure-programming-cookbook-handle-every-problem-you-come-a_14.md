# Chapter 10. Deployment and DevOps

在本章中我们将涵盖以下主题：

- - Riemann - monitoring deliverance and slacking
- - Deploying Clojure with Docker
- - Clojure on Amazon Web Services

## Introduction

本章将重点介绍部署和监控你的 Clojure 代码。第一部分将深入探讨使用 Docker 容器在本地、远程以及平台即服务上部署 Clojure 代码。然后你将了解如何使用名为 Riemann 的 Clojure 编写的监控工具来监控你的应用程序，以及由此扩展的运行代码的服务器。

最后，我们将了解如何使用 AWS 服务，包括 EC2、S3 和 Lambda。

## Riemann - monitoring deliverance and slacking

在生产中使用监控似乎总是很麻烦。唯一重要的事情是在你想要的时候以你想要的方式获取信息，就像你需要每天服用 Clojure 剂量时可以拿起这本书一样。

在本教程中，我们将介绍 Riemann，http://riemann.io，一个基于流的监控框架。Riemann 带有易用的客户端 API、简单的设置、交互式仪表板，以及与 Slack 等其他通知框架的简单交互。

Riemann 服务的核心是用 Clojure 编写并用 Clojure 配置。通过流概念进行事件处理感觉和行为都非常函数式。

最终，将其为你的 Raspberry Pi 集群设置并实时收集物联网知识也应该变得轻而易举。

## Getting ready

本教程与其他教程有点不同，因为主要交互点是一个服务，因此我们将快速介绍安装守护进程、配置它、准备客户端以及查看仪表板。

## Installing the Riemann service

该软件包作为多种平台的安装程序提供。Raspberry Raspian 和其他一些 Linux 发行版的默认安装程序是 deb 文件，可以从 Riemann 网站 http://riemann.io 下载：

```
dpkg -I riemann_0.2.11_all.deb
```

一旦 Riemann 安装完成，你可以使用以下命令启动它：

```
sudo systemctl start riemann
```

你也可以使用通常的 systemd 命令控制守护进程。

启动守护进程是成功的一半，因此我们还将快速查看为远程访问配置守护进程的步骤。

## Configuring the Riemann daemon

虽然许多更复杂的配置细节将在本教程后面介绍，但让我们回顾一下配置守护进程的基础知识。

默认配置文件位于
/etc/riemann/riemann.config，在其原始形式下相当简短。
这会将守护进程日志放在 /var/log 文件夹中，并指示它们侦听 TCP、UDP 和 WebSocket 连接（是的！Websockets...）：

```
(logging/init {:file "/var/log/riemann/riemann.log"})

; Listen over TCP (5555), UDP (5555), and websockets
; (5556)
(let [host "0.0.0.0"]
  (tcp-server {:host host})
  (udp-server {:host host})
  (ws-server  {:host host}))
```

请注意，默认主机不是 0.0.0.0，而是 localhost，因此在继续之前请确保更改它，以便你可以从远程机器发送事件。

## The Riemann dashboard

仪表板本身是用 Ruby 语言使用 Sinatra 框架开发的，但我确信你很快就会用 Clojure 和 ClojureScript 实现一个 Riemann 仪表板。

现在，开始安装仪表板，打包为 Ruby gem：



```
sudo gem install riemann-dash --no-ri --no-rdoc
```

然后启动仪表板本身：

```
riemann-dash
```

现在可以通过 http://localhost:4567/ 访问：

![](img/d7b631352a782ee50160cf8eb995152f_641_0.png)

我们将在几分钟后查看如何填充它，但首先让我们向 Riemann 守护进程发送一些事件。

## Riemann-health

默认仪表板默认情况下感觉有点空，看到这么多白茫茫的感觉有点难过，所以让我们快速看看如何为单个主机添加详细信息。

Riemann 有一个简单的脚本可以启动来发送主机的重要信息。安装是通过 gem install 完成的，使用以下命令：

```
sudo gem install riemann-tools
```

然后可以用以下方式启动脚本：

```
riemann-health --host riemann-host
```

在前面的命令中，`riemann-host` 是运行 Riemann 守护进程的机器的主机名或 IP。

好吧，现在我们已经有守护进程向 Riemann 主机发送事件了，但你最终希望可视化它们。

## 足够的仪表板配置

仪表板可以看作是一组带有每个标签页中小部件的标签。

在每个标签页中，要进入选择模式，按 *Control/Meta* + 单击选择一个小部件，或在 Mac 上按 *Option* + *Command* + 单击。

如果操作正确，视图会变色。在下面的屏幕截图中，底部的小部件被选中并变为深灰色：

## Riemann

欢迎使用 Riemann-Dash。

需要查询语言的复习？查看查询测试以获取示例，或阅读规范。

双击工作区以重命名它。

按 Control/Meta+单击选择视图（Mac 上为 Option+Command+单击）。Escape 取消聚焦。使用箭头键移动视图。使用 Control+箭头在给定方向上拆分视图。

要编辑视图，按 e。使用 enter，或单击“apply”，应用您的更改。Escape 取消。

要将更改保存到服务器，按 s。要显示配置，按 c。

然后你可以通过使用箭头键移动该小部件：

欢迎使用 Riemann-Dash。

需要查询语言的复习？查看查询测试以获取示例，或阅读规范。

双击工作区以重命名它。

按 Control/Meta+单击选择视图（Mac 上为 Option+Command+单击）。Escape 取消聚焦。使用箭头键移动视图。使用 Control+箭头在给定方向上拆分视图。

要编辑视图，按 e。使用 enter，或单击“apply”，应用您的更改。Escape 取消。

要将更改保存到服务器，按 s。要显示配置，按 c。

## Riemann

最终，你可以通过按 e 键编辑其内容。在这里，我们只想查看主服务器的健康状况，在本例中是运行 riemann-health 的机器。

我们将使用 flot 小部件，并避免监控 Riemann 守护进程正在工作的机器，在本示例中为 Debian-jessie-amd64-netboot：

![](img/d7b631352a782ee50160cf8eb995152f_644_0.png)

## 注意

注意：查询似乎不能为空，因此如果稍后仪表板上没有显示数据，请确保您有一个查询。

然后你可以通过按 Esc 两次退出小部件编辑，第二次退出小部件选择模式。你应该得到如下所示的内容：

![](img/d7b631352a782ee50160cf8eb995152f_645_0.png)

几乎不需要任何努力，你已经可以从系统中获得实时数据。

如果你想限制从中接收的数据量，将查询更新为仅显示，例如 CPU 使用率：

```
host != "Debian-jessie-amd64-netboot"  and service = "cpu"
```

![](img/d7b631352a782ee50160cf8eb995152f_646_0.png)

当你对仪表板的布局感到满意时，按 s 键将配置持久化到磁盘。

在继续之前，请确保你好好玩玩这个小部件！

## 准备发送事件

最后，为了准备好发送一些事件，我们将使用 Clojure 的 Riemann 客户端。因此，我们必须将 Riemann 客户端添加到新 Clojure 项目的 project.clj 文件中。你稍后还会玩一些 Slack 集成，所以让我们添加一个库来帮助我们做到这一点。依赖项应如下所示：

```
:dependencies [
  [riemann-clojure-client "0.4.2"]
  [slack-rtm "0.1.3"]
  [org.clojure/clojure "1.8.0"]]
```

现在，让我们飞往监控岛！

## 如何操作...

现在我们已经设置好并准备就绪，你将开始发送一些报告事件。

## 服务的基本事件报告

该片段是通常的、看似简短的 Clojure 片段，在此阶段你将只有三行代码：

```
(require '[riemann.client :as r])
(def c (r/tcp-client {:host "riemann-host"}))
(-> c (r/send-event {:service "first-service" :state "ok"}))
```

让我们分解前面的代码片段：

- 第一条线是一个简单的命名空间 `require`
- 第二条线获取一个 `tcp-client`（你也可以在这里使用 UDP）
- TCP 客户端会自行重新连接到 Riemann 守护进程
- 最后一行执行事件发送

事件在这里简单地由 `:service` 和 `:state` 组成。回到我们之前使用的仪表板。让我们添加一些内容来看看新服务的外观：

![](img/d7b631352a782ee50160cf8eb995152f_647_0.png)

而且……它很空：

![](img/d7b631352a782ee50160cf8eb995152f_648_0.png)

缺少什么？也许……是的！让我们给这个事件一个值。这是通过 metric 参数完成的：

```
(-> c (r/send-event
  {:service "first-service" :state "ok" :metric 10}))
```

随着 metric 本身的少量变化，你最终可能会得到一些不错的绘图，并且运气好的话它看起来会与以下相同：

![](img/d7b631352a782ee50160cf8eb995152f_649_0.png)

事件映射中包含的两个重要键是：

- `:description`，将在日志视图中显示
- `:tag`，在任何搜索中都很有用

切换到日志小部件，如下所示：

## 编辑日志

日志

标题 主机

查询

```
service = "first-service"
```

最大行数（事件数） 1000

应用

切换并发送描述后，你将看到以下内容：

| Host | service | state | metric | description |
| host |  |  |  |  |
| Nicolass-MacBook.local | first-service | ok | 100 | first description |

此方法非常有助于帮助你跟踪不同的服务。就这样。继续之前请确保你尝试使用标签和搜索。

## 事件过期

如果你在阅读本文之前发送了一些事件（我确信你确实发送了）

你可能已经注意到值往往会消失。这是因为默认的 Riemann 守护进程配置会定期过期事件，如下所示。

快速查看位于 `/etc/riemann/riemann.config` 的 Riemann 配置文件，显示在底部：

```
(periodically-expire 5)

(let [index (index)]
  ; Inbound events will be passed to these streams:
  (streams
    (default :ttl 60
      ; Index all events immediately.
      index

      ; Log expired events.
      (expired
        (fn [event] (info "expired" event))))))
```

这会使事件在 60 秒 ttl 后过期，并发送一个从已过期事件派生的新过期事件。如果你仍在运行日志小部件，它将显示如下屏幕截图：

| Host | service | state | metric | description |
| host |  |  |  |  |
| Nicolass-MacBook.local | first-service | ok | 100 | first description |
| Nicolass-MacBook.local | first-service | expired |  |  |

这是你第一次接触 Riemann 流的概念，所以让我们看看它们是如何工作的。

## 它是如何工作的...

如 Riemann 文档所示，流通常执行以下一项或多项操作：

- 1. 过滤事件。
- 2. 转换事件。
- 3. 随时间组合事件。
- 4. 将事件应用于其他流。
- 5. 将事件转发到其他服务。

对于一个不错的简单示例，让我们看看如何将事件转发到另一项服务。

假设你想在其中一项服务进入奇怪状态时收到电子邮件。为此，你将执行以下步骤：

- 1. 使用电子邮件设置配置 Riemann 系统。
- 2. 在 Riemann 配置中定义一个流，过滤状态设置为 error 的事件。
- 3. 将选定的事件转发到电子邮件。

好吧！电子邮件配置本身在幕后使用 Clojure postal 库，因此 Google Mail 设置看起来像这里显示的绑定：

```
(def email (mailer {:from "riemann@gmail.com"
:host "smtp.gmail.com"
:ss1 :yes!!!!11
:user "yourusername"
:pass "yourpassword"}))
```

前面片段中唯一令人惊讶的部分是 mailer，你实际上可以将其视为邮件流。你传递给它的所有内容都将被邮件发送。

其余配方现在专注于使用状态设置为 error 的事件过滤事件。这是通过流完成的，流通过基于查询的过滤创建事件流。

在这里，查询是选择状态设置为 error 的事件：

```
(streams (where (state "error")
...))
```





现在尝试填空吧！你可以在由所选事件组成的父流中使用 mailer 流：

```
(streams
  (where (state "error")
    (email "seriouserror@gmail.com")))
```

我们已经看过如何发送事件，所以下面是重述内容，并将发送一个 state 设置为 error 的事件，以及一些额外的描述：

```
(-> c (r/send-event {:service "first-service" :state "error" :metric 100 :description "serious error description"}))
```

如果你正确支付了邮件提供商，你会得到类似以下的内容：

![](img/d7b631352a782ee50160cf8eb995152f_653_0.png)

如果事件超级紧急怎么办？这就是“There's more”部分发挥作用的地方！

## There's more...

在本节的最后部分，你将确保通过 Riemann 传输的极其重要的通知能够正确转发到你的 Slack 账户，反之亦然；也就是说，Riemann 消息可以通过机器人转发并通过 Riemann 进行监控。

## 从 Riemann 到 Slack webhooks

要将数据导入 Slack，我们将创建一个自定义 webhook。任何通过给定 URL 发布的内容都会进入 Slack 频道。

从 Slack 网站，在 Build 部分，点击此处显示的按钮：

![](img/d7b631352a782ee50160cf8eb995152f_654_0.png)

然后选择要发送事件到的频道：

![](img/d7b631352a782ee50160cf8eb995152f_654_1.png)

注册后，你将获得如下所示的 URL：
https://hooks.slack.com/services/T04692339/B1X2ECBCJ/1iYCVuXc9b

Webhook URL 你所获得的最后一个 / 之后的部分就是你在 Riemann 配置中需要用作 token 的内容——因此，在本例中，token 是 1iYCVu....

这给出了此处所示的配置和 token：

```
(def credentials
  {:account "your_slack_account",
   :token "1iYCVuXc9bsxZHBNPpeBDtR1"})

(def slacker
  (slack credentials
    {:username "webhookbot"
     :channel "#monitoring"
     :icon ":smile:"}))
```

Slack 集成已准备就绪，现在让我们在关键事件上进行过滤并将事件转发到 Slack：

```
(streams
  (where (state "critical")
  slacker))
```

原始事件的发送与平常大致相同，但我们的 first-service 出现问题了！

```
(-> c (r/send-event {:service "first-service" :state "critical" :metric 100 :description "first description"}))
```

这就是它在 Slack 中的外观：

## Riemann Event

Host: Nicolass-MacBook.local
Service: first-service
State: critical
Description: first description
Metric: 100
Tag: -

Woohoo! 超级 Slack 能力！

## 从 Slack 机器人到 Riemann

第二个 Slack 示例使用 Slack 机器人来监听消息并将其转发到你的 Clojure 代码。

在 Slack 的 Build 部分，让我们寻找机器人：

![](img/d7b631352a782ee50160cf8eb995152f_656_0.png)

注册后，你最终会得到一个名称和一个 token：

### Customize Name

为此机器人选择用户名。

@breakfast

用户名必须为 al
包含字母。 numbe

一旦你的机器人注册，Clojure 就可以与之交互！你也可以使用 recipe 开始时设置的 rtm 库。

此处显示的片段会将发送到 breakfast 机器人的 Slack 事件写入文件。最困难的部分是使用 connect 函数并使用正确的回调设置：

```
(use 'slack-rtm.core)

(def token "bot-token")
(defn rcv-msg[evt]
  (spit "evt.log" (:message evt) :append true))

(def rtm-conn (connect token :on-receive rcv-msg))
```

显然，你走这么远不是为了写文件，而是为了学习 Riemann。代码只长那么一点点，而且是简单的混合，你已经见过：

```
(use 'slack-rtm.core)
(require '[riemann.client :as r])

; slack to riemann
(def c (atom (r/tcp-client {:host "riemann-host"})))

(def token "xoxb-64790720018-RhD3YOsLWJ4yc13GlX0CDeQe")
(defn rcv-msg[evt]
  (-> @c (r/send-event {:service "slack" :description (:message evt)})))
```

```
(def rtm-conn (connect token :on-receive rcv-msg))
```

就这样！现在没有借口不 slacking around 了。

## 使用 Docker 部署 Clojure

在当今的 IT 世界，很难不听到人们谈论 Docker。描述如何构建一致的镜像然后能够在不同服务器上运行的想法，使其成为组件化应用程序世界的一大进步。

在本 recipe 中，你将看到如何将标准的 Clojure Web 应用转变为 Docker 容器，从根本上确保你可以将代码分发到尽可能多的位置。

### Tip

这也是保持多台机器同步更新的好方法！使用 `latest` 作为标签，你也可以确保不同机器始终运行软件的最新版本。

## 准备工作

在本 recipe 中，我们假设有一个主要客人，运行 Mac OS 或 Windows 的开发机器，以及另一台运行 Debian 系统的机器。

## 在 OS X 或 Windows 上安装 Docker

截至撰写本文时，Docker for Mac 刚刚发布，使在你的机器上安装 Docker 变得非常容易；这是向前迈出的巨大一步。

https://docs.docker.com/engine/installation/mac/#/docker-for-mac

基本上，下载安装程序，运行安装程序……然后 Docker 管理图标显示在顶部栏中：

![](img/d7b631352a782ee50160cf8eb995152f_660_0.png)

## 在 Debian 上安装 Docker

在 Debian 上安装概念上很简单，但让我们再多做几步，总结在以下脚本中。

似乎系统虽然现在是默认的，但并不读取 /etc/default/docker，就像通常的情况一样，所以更新也包含在以下片段中：

```
# get super powers
sudo -s
# prepare sustem to accept https repositories
apt-get install apt-transport-https ca-certificates
apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
# add the docker repository
echo "deb https://apt.dockerproject.org/repo debian-jessie
```

```
main" >> /etc/apt/sources.list.d/docker.list
```

```
# install docker
apt-get update
apt-get install docker-engine
```

```
# configure docker for remote access
vi /lib/systemd/system/docker.service
# update line below with a tcp binding:
# ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2376
```

```
# reload sytemd deamon, to read the just changed docker.service file
systemctl daemon-reload
# restart docker
systemctl restart docker
```

暂时足够 Docker 设置了。让我们创建目标 Clojure 应用程序。

## 准备 Clojure 应用程序

虽然 Clojure 中有许多完全准备好的模板供我们使用，但本 recipe 中使用的应用程序将是一个从头创建的 compojure 应用。

使用以下命令创建一个新项目：

```
lein new withdocker
```

现在让我们稍微修改一下 project.clj 文件：

```
(defproject withdocker "0.1.0-SNAPSHOT"
  :dependencies [[org.clojure/clojure "1.8.0"]
                  [compojure "1.5.1"]
                  [ring/ring-jetty-adapter "1.3.2"]
                  [ring/ring-defaults "0.2.1"]]
  :main withdocker.handler
  :plugins [[lein-ring "0.9.7"]]
  :ring {:handler withdocker.handler/app}
  :profiles {:uberjar {:aot :all}
             :dev {:dependencies
[[javax.servlet/servlet-api "2.5"]
```

```
[ring/ring-mock "0.3.0"]]})
```

应用程序本身的完整代码可以在 `withdocker.handler` 命名空间中找到：

```
(ns withdocker.handler
  (:gen-class)
  (:require
    [ring.adapter.jetty :refer [run-jetty]]
    [compojure.core :refer :all]
    [compojure.route :as route]
    [ring.middleware.defaults
      :refer [wrap-defaults site-defaults]]))

(defroutes app-routes
  (GET "/" [] "Hello Docker World")
  (route/not-found "Not Found"))
(def app
  (wrap-defaults app-routes site-defaults))
(defn -main []
  (run-jetty app {:port 8080}))
```

正如你所见，应用程序可以通过 `lein` 在本地启动，使用：

```
lein run
```

打开 `http://localhost:8080` 页面会得到：

![](img/d7b631352a782ee50160cf8eb995152f_662_0.png)

`lein run` 调用将运行默认命名空间中的 `-main` 方法，该方法将启动一个嵌入式 jetty 服务器提供使用



defroutes macro.

## 将应用程序打包成 JAR 文件

你之前见过这个，但前面的应用程序也可以通过创建包含所有依赖项的 JAR 文件并运行该 JAR 文件的主类 main 方法来运行：

```
lein uberjar
java -jar target/withdocker-0.1.0-SNAPSHOT-standalone.jar
```

主入口点由 Leiningen 为你自动生成，通过生成位于 JAR 文件中的 META-INF/MANIFEST.MF 文件，其中 main-Class 条目取自 project.clj 中的 :main withdocker.handler：

```
Manifest-Version: 1.0
Built-By: niko
Created-By: Leiningen 2.6.1
Build-Jdk: 1.8.0_66
Main-Class: withdocker.handler
```

## 创建 Docker 容器

创建 JAR 文件并作为独立程序运行的思想实际上就是你将用来创建独立 Docker 镜像所做的事情。

我们在这里假设你之前见过 Dockerfile，但如果没有也不用担心！一切都很好。Dockerfile 就像好老的 Makefile 一样，描述了一系列步骤。Dockerfile 应放置在项目文件夹的根目录。

下面是一个带注释的 Dockerfile：

```
# a base image for the container, here something that has the java executable
FROM java:openjdk-8-jre
# copy the jar file inside the container to /app/withdocker.jar
COPY target/withdocker-0.1.0-SNAPSHOT-standalone.jar /app/withdocker.jar
# say the container will be exposing an app on port 8080
EXPOSE 8080
# what will be executed when you do, docker run ...
ENTRYPOINT [ "java", "-jar", "/app/withdocker.jar"]
```

假设你现在本地机器上已经运行了 Docker daemon，你可以构建名为 withdocker 的容器：

```
docker build . -t withdocker
```

如果一切顺利，你应该会看到一些很酷的输出：

```
Sending build context to Docker daemon 12.66 MB
Step 1 : FROM java:openjdk-8-jre
---> 76fd51ceaa2e
Step 2 : COPY target/withdocker-0.1.0-SNAPSHOT-standalone.jar /app/withdocker.jar
---> 10df21f2d7b2
Removing intermediate container 6444756f7aaf
Step 3 : EXPOSE 8080
---> Running in 0496cc294447
---> 56446e3f92fe
Removing intermediate container 0496cc294447
Step 4 : ENTRYPOINT java -jar /app/withdocker.jar
---> Running in 030972a7ac62
---> cea5a78eab27
Removing intermediate container 030972a7ac62
Successfully built cea5a78eab27
```

作为提醒，前面的输出中的每个步骤都对应 Dockerfile 中的一行。

现在让我们启动我们刚烤好的容器：

```
docker run withdocker
```

而且...Woohoo！Jetty 启动了！

```
2016-08-03 16:28:10.912:INFO:oejs.Server:jetty-7.x.y-SNAPSHOT
2016-08-03 16:28:11.257:INFO:oejs.AbstractConnector:Started SelectChannelConnector@0.0.0.0:8080
```

啊...正如你可能已经注意到的，页面不会打开。你仍然需要告诉 Docker 它需要映射的端口；在做这件事的同时，

让我们把它作为后台进程：

```
docker run -d -p 8080:8080 --name withdocker withdocker
```

这将告诉 Docker 将容器的 8080 端口映射到本地机器的 8080 端口，从而使其对外部世界可见。

Hello Docker World...

## 如何操作...

现在你已经制作了一个漂亮的 Docker 镜像，你准备好征服世界并将你的镜像部署到不同的地方。

正如你一些朋友之前所做的那样，通常第一个要攻克的据点是 Docker Hub，网址为 https://hub.docker.com/。

如果还没有，请在那里创建一个账户，然后回来继续。

从命令行，我们将首先登录账户，按照这里显示的小配方操作：

```
nicolassmacbook% docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username (hellonico):
Password:
Login Succeeded
```

### Note

请注意，期望的登录用户名是 Docker Hub ID，而不是使用的电子邮件地址。

构建 Docker 容器会稍微长一点，镜像名称前面加上前缀：

```
docker build . -t hellonico/withdocker
```

这个 hellonico 前缀将告诉 Docker 容器准备好将镜像推送到 Docker Hub。说到推送...

```
nicolassmacbook% docker push hellonico/withdocker
```

```
The push refers to a repository
[docker.io/hellonico/withdocker]
4ec68560f7e5: Pushing [===================>]
] 3.442 MB/7.456 MB
b5d7c200f6f3: Layer already exists
```

Docker 镜像现在可以在 Docker Hub 上找到：

![](img/d7b631352a782ee50160cf8eb995152f_666_0.png)

现在，由于这个配方假设你有一台设置了 docker 的第二台机器，让我们尝试在那里运行镜像。由于我们稍微更改了镜像名称以将其上传到 Docker Hub，命令现在如下所示：

```
docker run -d -p 8080:8080 --name withdocker
hellonico/withdocker
```

由于镜像未在本地构建，你会得到一些友好的提醒和一些免费下载：

```
Unable to find image 'hellonico/withdocker:latest' locally
latest: Pulling from hellonico/withdocker
5c90d4a2d1a8: Downloading
[============================================>          ] 40.89 MB/51.35 MB
```

除了花费几秒钟找到 docker IP 之外，这在那里也很好地打开了！

![](img/d7b631352a782ee50160cf8eb995152f_667_0.png)

很棒。现在变得有趣了。让我们看看如何跳过 Docker Hub 步骤并直接部署到远程机器。

## 直接连接到远程 Docker

运行任何 Docker 命令时，你可以要求 Docker 客户端连接到本地 daemon 或远程 daemon。

默认 daemon 运行在 Unix socket 上，通常位于 /var/run/docker.sock 附近。

如果你记得我们在 Debian 机器上做的配置更改，或者如果你还没有做，这里有一个快速提醒。你会看到我们要求 daemon 将自身绑定到 Unix socket 和 TCP socket：

```
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2376
```

Docker daemon 现在能够接收来自本地客户端或将使用 TCP 协议的远程 Docker 客户端的命令。

要要求客户端与远程协议通信，需要设置 DOCKER_HOST 属性并为其分配远程 Docker daemon 运行的位置的值：

```
export DOCKER_HOST="192.168.1.4:2376"
```

现在任何命令都将针对远程主机，因此运行简单的 ps 将显示在远程 daemon 上运行的容器：

```
docker ps
CONTAINER ID      IMAGE                     COMMAND
CREATED           STATUS                    PORTS
NAMES
556224b9435e      hellonico/withdocker      "java -jar
/app/withd"      9 minutes ago             Up 9 minutes
0.0.0.0:8080->8080/tcp    withdocker
```

所以现在，即使构建镜像也会在远程 daemon 上完成：

```
docker build . -t hellonico/withdocker
```

这太酷了。这意味着从代码仓库的不同分支部署代码到远程环境现在可以在几秒钟内完成：

```
lein uberjar && docker build . -t hellonico/withdocker
```

作为留给读者的练习，更新 withdocker/handler.clj 为更有趣的内容，再次构建容器，然后使用以下命令运行 Docker 镜像：

```
docker run -d -p 8081:8080 --name withdockerremote
hellonico/withdocker
```

新的镜像已被创建，现在运行在 8081 端口：

![](img/d7b631352a782ee50160cf8eb995152f_668_0.png)

## 还有更多...

最后，为了以一种有趣轻松的方式结束这个配方，我们将看到在远程 PAAS 上启动你的 Docker 镜像之一是多么容易。

我们在这里要看的提供商名为 [sloppy.io](sloppy.io)，但我们还没有与他们达成任何广告协议，所以请将其视为纯粹简单的介绍：

![](img/d7b631352a782ee50160cf8eb995152f_669_0.png)

一旦你获得了全新的、暂时免费的新账户，创建一个新项目：





## 基础设置

## 性能

## 卷

## 高级

## Docker 镜像

镜像路径

helloworld/withdocker

镜像标签

latest (default)

注意：如果您想使用私有仓库中的镜像，您首先需要提供您的 Docker Hub 凭证，否则部署将会失败，因为 sloppy.io 无法访问该镜像。

CMD 命令

输入可选 CMD 命令...

## 网络

域名 URI

http(s):// clojure2.sloppy.zone

暴露端口

Port #

注意您刚刚创建的镜像可以直接从 Docker Hub 拉取，以及您现在可以将最好的 sloppy 代码推送到野外：

![](img/d7b631352a782ee50160cf8eb995152f_671_0.png)

clojure.sloppy.zone

# Hello Remote Docker World

sloppy 的一些超级酷炫功能包括通过启动同一镜像的更多实例来实现按需性能：

![](img/d7b631352a782ee50160cf8eb995152f_672_0.png)

此外，重启容器将始终拉取最新的镜像，从而使从开发到生产的旅程几乎可以忽略不计：

![](img/d7b631352a782ee50160cf8eb995152f_672_1.png)

现在轮到你开发和部署了！

## Amazon Web Services 上的 Clojure

本教程是一个独立的菜谱，您可以在其中学习如何将 Clojure 的优雅与 Amazon Web Services (AWS) 相结合。

AWS 于 2006 年启动，并被许多企业用作易于使用的网络服务。这种无服务器服务风格变得越来越流行。您可以在无需自行准备硬件或安装软件的情况下，按需使用计算资源和软件服务。

您将主要使用 `amazonica` 库，它是整个 Amazon AWS API 集的综合 Clojure 客户端。该库封装了 Amazon AWS API，并支持大多数 AWS 服务，包括 EC2、S3、Lambda、Kinesis、Elastic Beanstalk、Elastic MapReduce 和 RedShift。

本教程的大部分内容和灵感来自 Robin Birtle，他是日本 Clojure 社区的主要成员。

## 准备工作

要使用 AWS，您需要一个 AWS 账户和凭证，因此本教程首先向您展示如何进行设置并获取开始所需的密钥。

## 在 AWS 上注册

如果您还没有 AWS 账户，您需要注册 AWS。在这种情况下，请访问 https://aws.amazon.com，点击 Sign In to the Console，然后按照创建账户的说明操作：

![](img/d7b631352a782ee50160cf8eb995152f_674_0.png)

要完成注册，请输入有效的信用卡号码和电话号码。

## 获取访问密钥和秘密访问密钥

要调用 AWS 客户端 API，您现在需要您的 AWS 访问密钥和秘密访问密钥。进入 AWS 控制台并点击屏幕右上角的名称，然后选择 Security Credentials，如以下屏幕截图所示：

选择 Access Keys (Access Key ID and Secret Access Key)，如以下屏幕截图所示：

# 您的安全凭证

使用此页面管理您的 AWS 账户的凭证。要管理 AWS Identity and Access Management (IAM) 用户的凭证，请使用 IAM 控制台。

要了解有关 AWS 凭证类型的更多信息以及它们的用途，请参阅 AWS General Reference 中的 AWS Security Credentials。

- Password
- Multi-Factor Authentication (MFA)
- Access Keys (Access Key ID and Secret Access Key)
- CloudFront Key Pairs
- X.509 Certificates
- Account Identifiers

然后，以下屏幕出现；点击 Create New Access Key：

![](img/d7b631352a782ee50160cf8eb995152f_676_0.png)

您可以查看您的访问密钥和秘密访问密钥，如以下屏幕截图所示：

![](img/d7b631352a782ee50160cf8eb995152f_677_0.png)

复制并保存这些字符串以供以后使用。

## 在 project.clj 中设置依赖项

让我们将 amazonica 库添加到您的 project.clj 并重启您的 REPL：

```
:dependencies [[org.clojure/clojure "1.8.0"]
[amazonica "0.3.67"]]
```

## 如何操作...

从这里开始，我们将通过 Clojure 和 amazonica 库访问的核心 Amazon 服务的一些示例用法。 我们将回顾的三个主要服务如下：

- 1. EC2，Amazon 的 Elastic Cloud，允许我们在 Amazon 云上运行虚拟机。
- 2. S3，Simple Storage Service，为您提供基于云的存储。
- 3. SQS，Simple Queue Services，为您提供基于云的数据

流式传输和处理。

让我们逐一介绍这些。

## 使用 EC2

假设您在东京区域有一个 EC2 微型实例：

![](img/d7b631352a782ee50160cf8eb995152f_678_0.png)

首先，我们将在 amazonica 中声明 core 和 ec2 命名空间：

```
(ns aws-examples.ec2-example
  (:require [amazonica.aws.ec2 :as ec2]
    [amazonica.core :as core]))
```

我们将设置访问密钥和秘密访问密钥以启用 AWS 客户端 API 访问。 core/defcredential 如下所示执行此操作：

```
(core/defcredential "Your Access Key" "Your Secret Access Key" "your region")
;;=> {:access-key "Your Access Key", :secret-key "Your Secret Access Key", :endpoint "your region"}
```

您需要指定的区域是 ap-northeast-1、ap-south-1 或 us-west-2。要获取完整的区域列表，请使用 ec2/describe-regions：

```
(ec2/describe-regions)
;;=> {:regions [{:region-name "ap-south-1", :endpoint "ec2.ap-south-1.amazonaws.com"}
;;=> .....
;;=> {:region-name "ap-northeast-2", :endpoint "ec2.ap-northeast-2.amazonaws.com"}
;;=> {:region-name "ap-northeast-1", :endpoint "ec2.ap-northeast-1.amazonaws.com"}
;;=> .....
;;=> {:region-name "us-west-2", :endpoint "ec2.us-west-2.amazonaws.com"}]}
```

ec2/describe-instances 返回大量信息：

```
(ec2/describe-instances)
;;=> {:reservations [{:reservation-id "r-8efe3c2b", :requester-id "226008221399",
;;=> :owner-id "182672843130", :group-names [], :groups [],
....
```

为了仅获取实例的必要信息，我们定义了以下 `get-instances-info`：

```
(defn get-instances-info[]
  (let [inst (ec2/describe-instances)]
    (->>
      (mapcat :instances (inst :reservations))
      (map
        #(vector
          [:node-name (->> (filter (fn [x] (= (:key x)) "Name") (:tags %)) first :value)]
          [:status (get-in % [:state :name])]
          [:instance-id (:instance-id %)]
          [:private-dns-name (:private-dns-name %)]
          [:global-ip (-> % :network-interfaces first :private-ip-addresses first :association :public-ip)]
          [:private-ip (-> % :network-interfaces first :private-ip-addresses first :private-ip-address)]))
        (map #(into {} %))
        (sort-by :node-name))))
;;=> #'aws-examples.ec2-example/get-instances-info
```

让我们尝试使用以下函数：

```
(get-instances-info)
;;=> ({:node-name "ECS Instance - amazon-ecs-cli-setup-my-cluster",
;;=> :status "running",
;;=> :instance-id "i-a1257a3e",
;;=> :private-dns-name "ip-10-0-0-212.ap-northeast-1.compute.internal",
;;=> :global-ip "54.199.234.18",
;;=> :private-ip "10.0.0.212"}
;;=> {:node-name "EcsInstanceAsg",
;;=> :status "terminated",
;;=> :instance-id "i-c5bbef5a",
;;=> :private-dns-name "",
;;=> :global-ip nil,
;;=> :private-ip nil})
```

如前面的示例函数所示，我们可以获取 instance-id 列表。因此，我们可以使用 ec2/start-instances 和 ec2/stop-instances 相应地启动/停止实例：

```
(ec2/start-instances :instance-ids '("i-c5bbef5a"))
;;=> {:starting-instances
;;=> [{:previous-state {:code 80, :name "stopped"},
;;=> :current-state {:code 0, :name "pending"},
;;=> :instance-id "i-c5bbef5a"}]}
(ec2/stop-instances :instance-ids '("i-c5bbef5a"))
;;=> {:stopping-instances
;;=> [{:previous-state {:code 16, :name "running"},
;;=> :current-state {:code 64, :name "stopping"},
;;=> :instance-id "i-c5bbef5a"}]}
```

## 使用 S3

Amazon S3 是 AWS 云中安全、耐用且可扩展的存储。对于开发人员和其他用户来说使用起来很容易。S3 还提供高耐用性、可用性和低成本。耐用性为 99.999999999 %，可用性为 99.99 %。

让我们按如下方式创建名为 makoto-bucket-1、makoto-bucket-2 和 makoto-bucket-3 的 s3 存储桶：

```
(s3/create-bucket "makoto-bucket-1")
```





```
; ;=> {:name "makoto-bucket-1"}
(s3/create-bucket "makoto-bucket-2")
; ;=> {:name "makoto-bucket-2"}
(s3/create-bucket "makoto-bucket-3")
; ;=> {:name "makoto-bucket-3"}
```

s3/list-buckets 返回存储桶信息：

```
(s3/list-buckets)
; ;=> [{:creation-date #object[org.joda.time.DateTime 0x6a09e119 "2016-08-01T07:01:05.000+09:00"],
; ;=> :owner
; ;=> {:id "3d6e87f691897059c23bcfb88b17da55f0c9aa02cc2a44e461f1594337059d27",
; ;=> :display-name "tokoma1"},
; ;=> :name "makoto-bucket-1"}
; ;=> {:creation-date #object[org.joda.time.DateTime 0x7392252c "2016-08-01T17:35:30.000+09:00"],
; ;=> :owner
; ;=> {:id "3d6e87f691897059c23bcfb88b17da55f0c9aa02cc2a44e461f1594337059d27",
; ;=> :display-name "tokoma1"},
; ;=> :name "makoto-bucket-2"}
; ;=> {:creation-date #object[org.joda.time.DateTime 0x4d59b4cb "2016-08-01T17:38:59.000+09:00"],
; ;=> :owner
; ;=> {:id "3d6e87f691897059c23bcfb88b17da55f0c9aa02cc2a44e461f1594337059d27",
; ;=> :display-name "tokoma1"},
; ;=> :name "makoto-bucket-3"}]
```

我们可以看到 AWS 控制台中有三个存储桶，如下图所示：

![](img/d7b631352a782ee50160cf8eb995152f_682_0.png)

让我们按如下方式删除其中两个存储桶：

```
(s3/delete-bucket "makoto-bucket-2")
;;=> nil
(s3/delete-bucket "makoto-bucket-3")
;;=> nil
(s3/list-buckets)
;;=> [{:creation-date #object[org.joda.time.DateTime 0x56387509 "2016-08-01T07:01:05.000+09:00"],
;;=> :owner {:id "3d6e87f691897059c23bcfb88b17da55f0c9aa02cc2a44e461f1594337059d27", :display-name "tokoma1"}, :name "makoto-bucket-1"}]
```

我们现在可以看到只有一个存储桶，如下图所示：

![](img/d7b631352a782ee50160cf8eb995152f_682_1.png)

现在我们将演示如何将本地数据发送到 s3。s3/put-object 将文件内容上传到指定的存储桶和键。以下代码上传 /etc/hosts 到 makoto-bucket-1：

```
(s3/put-object
:bucket-name "makoto-bucket-1"
:key "test/hosts"
:file (java.io.File. "/etc/hosts"))
;;=> {:requester-charged? false, :content-md5 "HkBljfktNT106yScnMRsjA==",
;;=> :etag "1e40658df92d353974eb249c9cc46c8c", :metadata {:content-disposition nil,
;;=> :expiration-time-rule-id nil, :user-metadata nil,
:instance-length 0, :version-id nil,
;;=> :server-side-encryption nil, :etag "1e40658df92d353974eb249c9cc46c8c", :last-modified nil,
;;=> :cache-control nil, :http-expires-date nil, :content-length 0, :content-type nil,
;;=> :restore-expiration-time nil, :content-encoding nil,
:expiration-time nil, :content-md5 nil,
;;=> :ongoing-restore nil}}
```

s3/list-objects 按如下方式列出存储桶中的对象：

```
(s3/list-objects :bucket-name "makoto-bucket-1")
;;=> {:truncated? false, :bucket-name "makoto-bucket-1", :max-keys 1000, :common-prefixes [],
;;=> :object-summaries [{:storage-class "STANDARD", :bucket-name "makoto-bucket-1",
;;=> :etag "1e40658df92d353974eb249c9cc46c8c",
;;=> :last-modified #object[org.joda.time.DateTime 0x1b76029c "2016-08-01T07:01:16.000+09:00"],
;;=> :owner {:id "3d6e87f691897059c23bcfb88b17da55f0c9aa02cc2a44e461f1594337059d27",
;;=> :display-name "tokomal1"}, :key "test/hosts", :size 380}]}
```

要获取存储桶中对象的内容，请使用 s3/get-object：

```
(s3/get-object :bucket-name "makoto-bucket-1" :key "test/hosts")
;;=> {:bucket-name "makoto-bucket-1", :key "test/hosts",
;;=> :input-stream #object[com.amazonaws.services.s3.model.S3ObjectInputStream 0x24f810e9
```
```
; ;=> .....
; ;=> :last-modified #object[org.joda.time.DateTime 0x79ad1ca9 "2016-08-01T07:01:16.000+09:00"],
; ;=> :cache-control nil, :http-expires-date nil, :content-length 380, :content-type "application/octet-stream",
; ;=> :restore-expiration-time nil, :content-encoding nil,
:expiration-time nil, :content-md5 nil,
; ;=> :ongoing-restore nil}
```

结果是一个 map；内容是流数据，值为 :object-content。要将结果作为字符串获取，我们将按如下方式使用 slurp_：

```
(slurp (:object-content (s3/get-object :bucket-name "makoto-bucket-1" :key "test/hosts")))
; ;=> "127.0.0.1\tlocalhost\n127.0.1.1\tphenix\n\n# The following lines are desirable for IPv6 capable hosts\n::1 ip6-localhost ip6-loopback\nfe00::0 ip6-localnet\nff00::0 ip6-mcastprefix\nff02::1 ip6-allnodes\nff02::2 ip6-allrouters\n\n52.8.30.189 my-cluster01-proxyl \n52.8.169.10 my-cluster01-master1 \n52.8.198.115 my-cluster01-slave01 \n52.9.12.12 my-cluster01-slave02\n\n52.8.197.100 my-node01\n"
```

## 使用 Amazon SQS

Amazon SQS 是一种高性能、高可用性和可扩展的队列服务。我们将演示使用 Clojure 处理 SQS 队列中的消息是多么容易：

```
(ns aws-examples.sqs-example
  (:require [amazonica.core :as core]
            [amazonica.aws.sqs :as sqs]))
```

要创建队列，可以按如下方式使用 sqs/create-queue：

```
(sqs/create-queue :queue-name "makoto-queue"
  :attributes
  {:VisibilityTimeout 3000
   :MaximumMessageSize 65536
   :MessageRetentionPeriod 1209600
   :ReceiveMessageWaitTimeSeconds 15})
; ;=> {:queue-url "https://sqs.ap-northeast-1.amazonaws.com/864062283993/makoto-queue"}
```

要获取队列信息，请按如下方式使用 sqs/get-queue-attributes：

```
(sqs/get-queue-attributes "makoto-queue")
;;=> {:QueueArn "arn:aws:sqs:ap-northeast-
1:864062283993:makoto-queue", ...
```

您可以使用 sqs/assign-dead-letter-queue 按如下方式配置死信队列（由针对不存在的队列的消息组成的队列）：

```
(sqs/create-queue "DLQ")
;;=> {:queue-url "https://sqs.ap-northeast-
1.amazonaws.com/864062283993/DLQ"}
(sqs/assign-dead-letter-queue (sqs/find-queue "makoto-queue")
(sqs/find-queue "DLQ") 10)
;;=> nil
```

让我们列出已定义的队列：

```
(sqs/list-queues)
;;=> {:queue-urls
;;=> ["https://sqs.ap-northeast-
1.amazonaws.com/864062283993/DLQ"
;;=> "https://sqs.ap-northeast-
1.amazonaws.com/864062283993/makoto-queue"]}
```

以下截图显示了 SQS 控制台：

![](img/d7b631352a782ee50160cf8eb995152f_682_1.png)

## 让我们检查队列的 URL：

```
(sqs/find-queue "makoto-queue")
;;=> "https://sqs.ap-northeast-1.amazonaws.com/864062283993/makoto-queue"
(sqs/find-queue "DLQ")
;;=> "https://sqs.ap-northeast-1.amazonaws.com/864062283993/DLQ"
```

要发送消息，我们使用 sqs/send-message：

```
(sqs/send-message (sqs/find-queue "makoto-queue") "hello sqs from Clojure")
;;=> {:md5of-message-body "00129c8cc3c7081893765352a2f71f97", :message-id "690ddd68-a2f6-45de-b6f1-164eb3c9370d"}
```

要接收消息，我们使用 sqs/receive-message：

```
(sqs/receive-message "makoto-queue")
;;=> {:messages [
;;=>     {:md5of-body "00129c8cc3c7081893765352a2f71f97",
;;=>      :receipt-handle "AQEB.....", :message-id "bd56fea8-4c9f-4946-9521-1d97057f1a06",
;;=>      :body "hello sqs from Clojure"}]}

```

要删除队列中的所有消息，我们使用 sqs/purge-queue：

```
(sqs/purge-queue :queue-url (sqs/find-queue "makoto-queue"))
;;=> nil

```

要删除队列，我们使用 sqs/delete-queue：

```
(sqs/delete-queue "makoto-queue")
;;=> nil
(sqs/delete-queue "DLQ")
;;=> nil

```

## 使用 AWS Lambda 的无服务器 Clojure

Lambda 是 AWS 的产品，允许您在无需设置和维护服务器环境的情况下运行 Clojure 代码。幕后仍然有服务器，但就您而言，它是一个无服务器环境。上传一个 JAR 即可开始使用。Lambda 上的代码会响应事件调用，例如文件上传到 S3，或根据指定的计划调用。在生产环境中，Lambda 通常用于包含标准服务器环境以处理离散计算任务的更广泛的 AWS 部署中。

特别是，它可以根据并发请求非常轻松地扩展。对于从事个人项目的 Clojurian 来说，Lambda 是功能与限制的绝佳结合。鉴于 AWS 设定的限制，您能将 Lambda 发挥到什么程度？

## Clojure 命名空间 helloworld

从使用 lein new 生成的干净空项目开始。从那里，在您选择的 IDE 中配置并打包一个新的 Clojure 源文件。在以下示例中，包是 com.sakkam，



source file uses the Clojure namespace helloworld. The entry point to your Lambda code is a Clojure function that is exposed as a method of a Java class using Clojure's gen-class.

Similar to use and require, the gen-class function can be included in the Clojure ns definition, as in the following, or specified separately. You can use any name you want for the handler function but the prefix must be a hyphen unless an alternate prefix is specified as part of the :methods definition:

```
(ns aws-example.lambda-helloworld
  (:gen-class
    :methods [^:static [handler [String] String]]))
(defn -myhandler [s]
  (println (str "Hello," s)))
```

From the command line, use lein uberjar to create a JAR that can be uploaded to AWS Lambda.

## Hello World - the AWS part

Getting your Hello World to work is now a matter of creating a new Lambda within AWS, uploading your JAR, and configuring your handler.

## Hello Stream

The handler method we used in our Hello World Lambda function was coded directly and could be extended to accept custom Java classes as part of the method signature. However, for more complex Java integrations, implementing one of AWS's standard interfaces for Lambda is both straightforward and feels more like idiomatic Clojure. The following example replaces our own definition of a handler method with an implementation of a standard interface that is provided as part of the aws-lambda-java-core library. First of all, add the dependency [com.amazonaws/aws-lambda-java-core "1.0.0"] into your project.clj. While you are modifying your project.clj, also add in the dependency for [org.clojure/data.json "0.2.6"] since we will be manipulating JSON-formatted objects as part of this exercise. Then,

either create a new Clojure namespace or modify your existing one so that it looks like the following (the handler function must be named - handleRequest since handleRequest is specified as part of the interface):

```
(ns aws-examples.lambda-example
  (:gen-class
    :implements
    [com.amazonaws.services.lambda.runtime.RequestStreamHandler])
  (:require [clojure.java.io :as io]
            [clojure.data.json :as json]
            [clojure.string :as str]))
(defn -handleRequest [this is os context]
  (let [w (io/writer os)
        parameters (json/read (io/reader is) :key-fn keyword)]
    (println "Lambda Hello Stream Output ")
    (println "this class: " (class this))
    (println "is class:" (class is))
    (println "os class:" (class os))
    (println "context class:" (class context))
    (println "Parameters are " parameters))
    (.flush w)))
```

Use lein uberjar again to create a JAR file. Since we have an existing Lambda function in AWS, we can overwrite the JAR used in the Hello World example. Since the handler function name has changed, we must modify our Lambda configuration to match. This time, the default test that provides parameters in JSON format should work as-is.

We can very easily get a more interesting test of Hello Stream by configuring this Lambda to run whenever a file is uploaded to S3. At the Lambda management page, choose the Event Sources tab, click on Add Event, and choose an S3 bucket to which you can easily add a file. Now, upload a file to the specified S3 bucket and then navigate to the logs of the Hello World Lambda function. You will find that Hello World has been automatically invoked, and a fairly complicated object that represents the uploaded file is supplied as a parameter to our Lambda function.

## Real-world Lambdas

To graduate from a Hello World Lambda to real-world Lambdas, the

chances are you are going to need richer integration with other AWS facilities. As a minimum, you will probably want to write a file to an S3 bucket or insert a notification into the SNS queue. Amazon provides an SDK that makes this integration straightforward for developers using standard Java. For Clojurians, using the Amazon Closure wrapper Amazonica is a very fast and easy way to achieve the same.

## How it works...

Here, we will explain how AWS works.

## What is Amazon EC2?

Using EC2, we don't need to buy hardware or install an operating system. Amazon provides various types of instance for customer use. Each instance type has various combinations of CPU, memory, storage, and networking capacity.

Some instance types are given in the following table. You can select appropriate instances according to the characteristics of your application.

| Instance type | Description |
|---|---|
| M4 | M4 类型实例专为通用计算而设计。该系列提供平衡的 CPU、内存和网络带宽 |
| C4 | C4 类型实例专为消耗 CPU 资源的应用而设计。C4 具有最低成本下的最高 CPU 性能 |
| R3 | R3 类型实例适用于内存密集型应用 |
| G2 | G2 类型实例具有 NVIDIA GPU，用于图形应用和 GPU 计算应用，如深度学习 |

The following table shows the variations of models of the M4 type instance. You can choose the best model.

| Model | vCPU | RAM (GiB) | EBS bandwidth (Mbps) |
|---|---|---|---|
| m4.large | 2 | 8 | 450 |
| m4.xlarge | 4 | 16 | 750 |
| m4.2xlarge | 8 | 32 | 1,000 |
| m4.4xlarge | 16 | 64 | 2,000 |
| m4.10xlarge | 40 | 160 | 4,000 |

## Amazon S3

Amazon S3 is Cloud storage. It provides a simple web interface that allows you to store and retrieve data. S3 API is easy to use but ensures security. S3 provides Cloud storage services and is scalable, reliable, fast, and inexpensive.

## Buckets and keys

Buckets are containers for objects stored in Amazon S3. Objects are stored in buckets. Bucket names are unique among all regions in the world. So, names of buckets are the top-level identities of S3, charge units, and access controls.

Keys are the unique identifiers for an object within a bucket. Every object in a bucket has exactly one key. Keys are the second-level identifiers and should be unique in a bucket. To identify an object, you use a combination of the bucket name and key name.

## Objects

Objects are accessed by a bucket name and keys. Objects consist of data and metadata. Metadata is a set of name-value pairs that describe the characteristics of object. Examples of metadata are the date last modified and content type. Objects can have multiple versions of data.

## There's no more...

It is clearly impossible to review all the different APIs for all the different services proposed via the Amazonica library, but you would probably get the feeling of having tremendous powers in your hands right now. (Don't forget to give that credit card back to your boss now...)

Some other examples of Amazon services are as follows:

- 1. Amazon IoT: This proposes a way to get connected devices to easily and securely interact with cloud applications and other devices.
- 2. Amazon Kinesis: This gives you ways of easily loading massive volumes of streaming data into AWS and easily analyzing them through streaming techniques.

## Table of Contents

Clojure Programming Cookbook 26

Credits 28

About the Authors 29

About the Reviewer 31

www.PacktPub.com 32

eBooks, discount offers, and more 32

Why subscribe? 32

Preface 33

What this book covers 33

What you need for this book 36

Who this book is for 37

Sections 38

Getting ready 38

How to do it... 38

How it works... 38

There's more... 38

See also 38

Conventions 39

Reader feedback 40

Customer support 41

Downloading the example code 41

Downloading the color images of this book 42

Errata 42

Piracy 42

Questions 43

1. Live Programming with Clojure 44

Introduction 44

REPL up! 45

Getting ready 45

How to do it... 45

- How it works... 48
- There's more... 49
- See also 51
- Working with primitive data types 52
- Getting ready 52
- How to do it... 52
- Using numbers 52
- Using strings and characters 54
- Using booleans and nil 56
- Using symbols and keywords 57
- How it works... 58
- There's more... 59
- Using bindings of vars, conditions, loops, and error handling 61
- Getting ready 61
- How to do it... 61
- def and let 61
- if and if-not 62
- when and when-not 63
- case and cond 64
- do and dotimes 65
- loop and recur 66
- try... catch... throw 67
- How it works... 67
- Using and defining functions 69
- Getting ready 69
- How to do it... 69
- Defining simple functions 69
- Defining variadic functions 70
- Defining multiple arity functions 70
- Defining functions that specify arguments using a keyword 71
- Defining functions with pre-condition and post-condition 71
- How it works... 73





## 2. 与集合交互 90
### 引言 90
### Clojure 集合及其基本函数 91
- 准备工作 91
- 如何操作... 91
- 列表 91
- 向量 92
- 映射 93
- 集合 95
- 如何工作... 96
- Clojure 集合 96
- 列表与向量之间的差异 97
- Clojure 是不可变的 98
- 还有更多... 99
- 在不同类型之间转换集合 99
- 使用 distinct 100
- 从集合中访问和更新元素 101
- 准备工作 101
- 如何操作... 101
- 使用 nth 函数访问集合 101
- 使用 get 访问映射或集合 102
- 映射、集合和关键字是用于访问集合的函数 102
- 使用 second、next、ffirst 和 nfirst 访问集合 103
- 使用 update 更新集合 104
- 如何工作... 104
- 还有更多... 105
- 对向量使用 get 105
- 使用集合作为映射中的键 105
- 使用 get-in 106
- 使用 assoc-in 106
- 使用 update-in 107
- 从集合中过滤元素 108
- 准备工作 108
- 如何操作... 108
- 过滤三的倍数 108
- 过滤映射的项目 108
- 过滤非 nil 值 109
- 从序列中移除奇数值 109
- 使用 keep 109
- 使用 keep-indexed 109
- 还有更多... 110
- 过滤作为转换器 110
- 使用 core.async 进行过滤 110
- 转换和排序集合 112
- 准备工作 112
- 如何操作... 112
- 使用内置 sort 112
- 迁移到 sort-by 113
- 使用 sorted-set 114
- 处理排序集合子集 115
- 排序 Clojure 映射 116
- 排序映射结构 117
- 你排序好了吗？ 118
- 如何工作... 119
- 还有更多... 120
- 排序文件行 120
- 合并和拆分集合 122
- 准备工作 122
- 如何操作... 122
- 使用 merge 和 merge-with 进行合并 122
- zipmap 合并映射的键和值 123
- 使用 interleave 123
- 使用 split-at 和 split-with 拆分集合 123
- 如何工作... 123
- 还有更多... 124
- 使用 group-by 拆分集合 124
- 使用 filter 函数拆分集合 124
- 如何变得懒惰 126
- 准备工作 126
- 如何操作... 127
- iterate 函数 127
- 已实现？ 128
- 使用 repeatedly 生成随机序列 128
- 使用 macro lazy-seq 128
- 如何工作... 129
- 还有更多... 130
- 其他产生 lazy-seq 的函数 130
- 使用 doall 强制求值 131
- 使用文件的惰性求值 131
- map 函数和调用 rest 132

## 3. Clojure Next 134
### 引言 134
### 使用解构技术 135
- 准备工作 135
- 如何操作... 135
- 序列解构 135
- 使用映射解构 136
- 如何工作... 138
- 还有更多... 138
- 字符串的序列解构 138
- 使用 core.match 进行条件解构 139
### 使用函数式编程风格 141
- 准备工作 141
- 如何操作... 141
- 函数接受函数作为其参数 141
- map 141
- reduce 142
- apply 142
- 使用匿名函数 143
- 使用 fn 的匿名函数 143
- 使用函数字面量定义匿名函数 144
- 函数返回函数 144
- constantly 144
- complement 144
- partial 145
- comp 146
- every-pred 和 some-fn 146
- 如何工作... 147
- Clojure 函数是第一类函数 147
- 纯函数 147
- 还有更多... 148
- reduce 和 apply 148
- loop 和 recur 148
### 使用线程宏 150
- 准备工作 150
- 如何操作... 150
- 介绍 -> 宏 150
- 介绍 ->> 宏 151
- 介绍 cond-> 和 cond->> 宏 152
- 介绍 some-> 和 some->> 154
- 如何工作... 155
- 还有更多... 156
- 介绍 as-> 156
- Flambo 预览 157
### 定义简单宏 160
- 准备工作 160
- 如何操作... 160
- 你的第一个宏 160
- 你的第二个宏 161
- 如何工作... 162
- 还有更多... 163
- 使用引号 164
- 到处都是宏 166
### 定义更高级的宏 168
- 准备工作 168
- 如何操作... 169
- 跟踪代码何时开始 170
- 使用宏记录消息 171
- 如何工作... 172
- 还有更多... 173
- deftest 173
### 使用协议 175
- 准备工作 175
- 如何操作... 175
- 定义记录 175
- 定义类型 177
- 定义协议 178
- 如何工作... 179
- 记录和类型是 Java 类 180
- 记录与类型之间的区别 180
- 还有更多... 182
- 定义可变协议和类型 182
### 定义多方法 184
- 准备工作 184
- 如何操作... 184
- 定义多方法 184
- 如何工作... 185
- 还有更多... 185
### 给初学者的转换器 187
- 准备工作 187
- 如何操作... 189
- 如何工作... 189
- 可组合函数 189
- 插入元素 190
- 编写自己的转换器 190
- 短路 192
- 还有更多... 193
- 使用 core.async 的转换器 193
- 总结 195
### Clojure 中的逻辑编程 196
- 准备工作 196
- 婴儿逻辑步骤 196
- 认识 "o" 词汇 197
- conso: 检查向量关联 197
- resto: 检查向量的其余部分 198
- membero 198
- appendo: 追加列表 199
- conde 199
- Matche 199
- 初学者逻辑 200
- 保持新鲜 200
- 仅查找一个结果 201
- 成熟逻辑 201
- 值范围 201
- 如何操作... 202
- 生成数据 202
- 使用更多逻辑变量生成更多数据 203
- 使用项目 203
- 如何工作... 204
- 还有更多... 206
- 验证函数的結果 207
- 使用外部数据 208

## 4. 文件访问和网络 210
### 引言 210
### 操作文件和目录 211
- 准备工作 211
- 如何操作... 211
- spit 和 slurp 211
- 使用 with-open 的读取器和写入器 212
- 创建文件和目录 212
- 复制文件 213
- 删除文件和目录 213
- 列出目录中的文件和目录 214
- 如何工作... 215
- slurp 和 spit 215
- with-open 宏 215
- 还有更多... 215
- 读取 HTTP 资源 216
- 检查源代码 216
### 操作各种格式的文件 - XML、JSON、YAML 和 EDN 220
- 准备工作 220
- 如何操作... 220
- XML 221
- JSON 222
- YAML 不是标记语言 224
- MessagePack，Clojure 库 225
- 如何工作... 228
- 还有更多... 230
- 可扩展数据表示法 230
### 使用 Clojure HTTP 客户端 233
- 准备工作 233
- 如何操作... 233
- 如何工作... 235
- 还有更多... 237
- async HTTP 和 core async 237
### 在 RabbitMQ 中使用队列和主题 239
- 准备工作 239
- 安装 RabbitMQ 239
- 如何操作... 241
- 生产 hello world 消息并消费它们 241
- 声明命名空间并加载 Langohir 库和 Docker 容器的 IP 地址 241
- 定义生产者函数 242
- 定义消息处理器和消费者 242
- 测试生产者和消费者 243
- 使用阻塞消费者 244
- 如何工作... 245
- 连接到代理并打开通道 245
- 生产者如何工作 245
- 声明队列 246
- 生产消息 246
- 消费者如何工作 246
- 消费者消息 247
- 定义消息处理器 247
- 还有更多... 248
- 使用 fanout 交换 248
- fanout-producer 248
- fanout-consumer 249
- 运行 fanout 消费者 249
- 测试 fanout 消费者 250
- 使用 topic 交换 250
- Topic-publisher 251
- Topic-consumer 252
- 运行 topic 消费者 252
- 测试 topic 消费者 253
- 另请参阅 254
### 使用 Kafka 255
- 准备工作 255





|  |  |
|---|---|
| 下载 Kafka 0.8.2.2 | 255 |
| 启动 ZooKeeper 和 Kafka 服务器 | 255 |
| 创建主题 | 256 |
| 运行控制台消费者 | 257 |
| 使用 clj-kafka | 257 |
| 如何操作... | 257 |
| 声明命名空间并加载 Clojure 库 | 258 |
| 生产消息 | 258 |
| 消费消息 | 258 |
| 它是如何工作的... | 259 |
| 生产消息 | 259 |
| 消费消息 | 260 |
| 还有更多... | 262 |
| 创建新主题 | 262 |
| 另请参阅 | 262 |
| 使用 MQTT | 264 |
| 准备就绪 | 264 |
| 安装 MQTT Broker | 265 |
| MQTT 图形客户端 | 265 |
| 发送我们的第一条 MQTT 消息 | 269 |
| 获取一些 Clojure | 272 |
| 如何操作... | 272 |
| 它是如何工作的... | 274 |
| 还有更多... | 276 |
| 流式访问以提供高性能 | 281 |
| 准备就绪 | 281 |
| 如何操作... | 281 |
| 流式输出 | 281 |
| 流式输入 | 283 |
| 使用 line-seq 进行流式处理 | 283 |
| 它是如何工作的... | 283 |

还有更多... 285
使用 Apache Camel 连接一切 287
准备就绪 287
如何操作... 287
导入 Camel 类 287
使用 Clojure 在 Camel 上的首次体验 287
使用 Camel 编写更符合 Clojure 习惯的代码 288
它是如何工作的... 290
还有更多... 290
另请参阅 291
## 5. 使用其他语言 292
### 简介 292
从 Clojure 调用 Java 方法并访问 Java 对象 293
准备就绪 293
如何操作... 293
实例化对象 293
调用实例方法 293
调用类方法 294
访问实例字段 294
访问类字段 295
访问内部类 295
引用类 296
使用数组 296
它是如何工作的... 297
Clojure 访问 Java 方法和对象的总结 298
Clojure 不继承 Java 的受检异常 299
数组访问的总结 300
打印 Java 原始类型的字符串 300
还有更多... 301
使用 double-dot 宏进行链式调用 301
使用 doto 宏 301

使用反射 302

扩展 Java 超类并实现 Java 接口 303

准备就绪 303

如何操作... 303

使用 proxy 303

使用 reify 实现接口 304

它是如何工作的... 304

还有更多... 305

在 Clojure 中定义匿名类 305

使用注解 306

从 Java 调用 Clojure 307

准备就绪 307

如何操作... 307

使用 gen-class 定义简单的命名类 307

在 Clojure 中定义带实例方法和构造函数的类 308

创建可从 Java 调用的 JAR 文件 310

它是如何工作的... 312

用于生成 Java 类的 project.clj 312

AOT 编译 313

还有更多... 313

定义可从 Java 调用的接口和实现类 314

使用 Maven 进行 Clojure 和 Java 项目 315

从 Clojure 调用 Scala 319

准备就绪 319

如何操作... 319

使用 Leiningen 开发 Scala 和 Clojure 319

导入 Scala 类 320

调用实例方法 320

调用单例方法 321

访问元组 321

访问 Scala 字段 321

它是如何工作的... 322
还有更多... 323
使用 SBT 323
创建基于 SBT 的项目 323
编写源代码 324
ClojureCLR 326
准备就绪 326
安装 Visual Code Studio 并创建项目 327
安装 Mono Studio IDE 332
Lein CLR 337
如何操作... 339
从 Clojure 调用 .NET 代码 339
启动 REPL 339
使用 IO 340
使用 C# 代码解析 XML 341
使用 Clojure/C# 调用 REST API 342
UI 原型设计 344
把所有东西整合在一起 346
它是如何工作的... 347
为 .NET 编译 Clojure 代码 347
将 Clojure 代码添加到 Visual Studio 项目中 349
使用 Clojure.RT 运行 Clojure 代码 351
使用 Xamarin Mono Studio 351
还有更多... 354
ClojureScript 357
准备就绪 357
如何操作... 359
自动编译你的代码 359
斐波那契 360
它是如何工作的... 360
与 JavaScript 交互 360
从 ClojureScript 使用 jQuery 361
创建 ClojureScript 库 364
使用第三方库 365
还有更多... 366
在 REPL 中运行 366
为 Clojure 和 ClojureScript 同时编译代码 367
## 6. 并发与并行 369
简介 369
使用 Clojure 解决并发问题 370
准备就绪 370
如何操作... 370
使用原子 370
创建和引用原子 370
更新原子 371
使用验证器 372
使用 CAS 操作 372
使用 ref 和 dosync 的软件事务内存 374
创建 ref 374
使用 alter 更新 refs 374
使用 ensure 376
使用 watcher 和改进的事务代码 377
使用 commute 379
使用代理 381
创建代理 381
更新代理 381
它是如何工作的... 382
Clojure 中三种引用类型的总结 382
协调/非协调 382
同步/异步 382
Clojure 中 STM 的工作原理 383
Alter 和 commute 383
还有更多... 383
promise 和 deliver 383
pmap、pcalls 和 pvalues 386
pcalls 386
pvalues 387
另请参阅 387
使用 Akka 进行基于分布式 Actor 的开发 388
准备就绪 389
如何操作... 389
你的第一个 okku 389
创建 Actor 390
分派消息 391
它是如何工作的... 392
Actor 系统 392
理解 Actor 393
还有更多... 394
调用远程 Actor 394
请求和回复 397
另请参阅 399
使用 Spyglass 和 Couchbase 在 JVM 之间共享状态 400
准备就绪 400
在 project.clj 中设置依赖项 400
将 Couchbase Server 设置为 Memcached 服务器 400
使用 docker-compose 启动 Couchbase Server 400
创建 docker-compose 的目录和文件 401
启动 Couchbase 集群 401
检查 Docker 进程是否正在运行 402
Couchbase 集群总结 403
使用 Couchbase Web 管理员控制台 403
登录 Web 控制台 403
向集群添加节点 405
添加集群 405
定义 Memcached 桶 408
如何操作... 410
定义使用 Spyglass 库 410
连接服务器 410
设置值 411
从另一个 REPL 获取值 411
测试生存时间 412
断开连接 412
它是如何工作的... 412
什么是 Memcached？ 412
从 Telnet 访问 Memcached 413
Couchbase Server 414
还有更多... 415
使用 get-multi 415
使用 async-get 416
使用 CAS 操作 416
另请参阅 420
使用 meltdown 进行响应式编程 421
准备就绪 421
如何操作... 422
定义一个 reactor 422
向 reactor 发送消息 423
走同步方式 423
它是如何工作的... 424
选择器 424
流 425
流和 reduce 426
使用图组合函数 427
自定义流 427
还有更多... 428
桥接 core.async 430
准备就绪 430
回忆 - 回顾 core.async 基础 431
发布/订阅 432
如何操作... 433
异步套接字服务器 433
异步套接字客户端 434
计时 435
还有更多... 436
客户端到 Python WebSockets 436
Clojure WebSocket 服务器 438
关于 Quasar/Pulsar 440
准备就绪 440
如何操作... 440
它是如何工作的... 445
监视其他 Actor 的生命周期 446
Actor 的状态 447
还有更多... 447
Blazar 448
## 7. 高级技巧 450
简介 450
破解 Clojure 代码 451
准备就绪 451
如何操作... 452
它是如何工作的... 454
还有更多... 456
使用 Reader 条件编译到 Clojure 和 ClojureScript 459
准备就绪 459
如何操作... 462
你的第一个 Reader Conditional 462
在命名空间中使用 Reader Conditionals 463
它是如何工作的... 464
Reader Conditionals 和 Splice 宏的形式 464
ClojureScript 中的宏 466

简介 510

Clojure 与 Vaadin - 简单的 Web 小部件 511

准备就绪 511

项目设置 511

Clojure/Vaadin 流程 512

如何操作... 515

使用日历小部件 515

对事件做出反应 517

点击按钮 518

使用和重新加载命名空间 519

服务器端推送 521

还有更多... 524

响应式 Vaadin - 想法 524

响应式 Vaadin - CPU 监控 526

响应式 Vaadin - Raspberry Pi 上的 Telegram 528

部署奖励 530

使用 Liberator 快速创建 REST API 531

准备就绪 531

如何操作... 532

你的第一个 Liberator 532

使用 defresource 534

参数化资源 535

为 REST API 定义 GET/POST/PUT/DELETE 方法 536

它是如何工作的... 540

HTTP 的方法和状态码 540

追踪请求 541

还有更多... 546

使用 clojure.java.jdbc 的持久 REST 资源 546

为 jdbc 做准备 546

我们的服务规范 547

创建产品表并定义访问函数 548




定义资源并启动服务器 550
从 HTTP 客户端访问 551
使用 Immutant - 重用基础设施 555
准备工作 555
如何操作... 556
使用 Artemis 进行简单消息传递 556
远程消息传递 559
使用队列进行远程过程调用 560
调度代码执行 562
管道和定义数据转换管道 562
使用 WebSockets 变得简单 563
更多内容... 565
打包为独立应用 566
打包为可部署的 Web 归档文件（WAR 文件） 566
使用 om.next 开发，下一代 ClojureScript 库 568
准备工作 568
为 om.next 创建新项目 568
更新 om 版本 568
依赖项提示 569
启动 figwheel 569
如何操作... 569
开始使用 om.next 570
使用状态 571
工作原理... 574
React 基础 574
defui 576
reconciler 576
组件生命周期 576
更多内容... 578
使用 om.next 的任务列表 578
定义命名空间和使用的库 578
定义状态 579
定义读取 579
定义变异 580
定义用户界面 582
9. 测试 588
介绍 588
行为驱动开发 589
准备工作 589
依赖项 589
第一个特性 589
设计文件夹结构以组织特性 590
如何操作... 592
使用表格进行大量假设 592
从 CSV 文件读取假设 594
使用 Excel 操作 595
使用 Web API 和 MongoDB 操作 597
更多内容... 599
VirtualBox 599
Freactive 600
Flambo，或 BDD 遇见 Apache Spark 601
EEP for BDD 602
OpenCV 603
使用随机输入和基于模式的测试进行测试 606
准备工作 606
库 606
生成随机值或值组 607
基于模式的随机值 609
如何操作... 610
你的第一个 test.check 规范 610
自动运行测试 611
更多规范和生成器 612
更多内容... 654
从 Riemann 到 Slack webhooks 655
从 Slack 机器人到 Riemann 657
使用 Docker 部署 Clojure 660
准备工作 660
在 OS X 或 Windows 上安装 Docker 660
在 Debian 上安装 Docker 661
准备 Clojure 应用 662
将应用打包为 JAR 文件 664
创建 Docker 容器 664
如何操作... 666
直接连接到远程 Docker 668
更多内容... 670
在 Amazon Web Services 上使用 Clojure 674
准备工作 674
在 AWS 上注册 674
获取访问密钥和秘密访问密钥 675
在 project.clj 中设置项目依赖项 678
如何操作... 678
使用 EC2 679
使用 S3 681
使用 Amazon SQS 685
使用 AWS Lambda 实现无服务器 Clojure 688
Clojure 命名空间 helloworld 688
Hello World - AWS 部分 689
Hello Stream 689
真实世界的 Lambdas 690
工作原理... 691
什么是 Amazon EC2？ 691
Amazon S3 692
存储桶和键 692
对象 692
