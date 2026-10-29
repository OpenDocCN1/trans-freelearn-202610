

;Ping
;Pong
;Ping
;Pong
;Ping
;Missed the ball. Ping finished
;Pong finished

这种乒乓来回很不错，但同意，如果在 spawn 时能避免把另一个乒乓者作为引用传递会更好。是的！这可以通过 register! 和一个关键字来实现。让我们重写 ping，使其能够使用关键字而不是引用发送消息：

```
(defsf n ping []
  (if (>= (rand-int 10) 9)
    (do
      (! :pong :finished)
      (println "Missed the ball. Ping finished"))
    (do
      (! :pong [:ping @self])
      (receive
        :pong (println "Ping"))
      (recur))))
```

现在，在开始游戏之前，我们使用 register! 将 pong 注册为 :pong 到 Quasar 系统，然后依次 spawn ping 和 pong。注意 ping 知道存在 :pong，并且需要它被注册：

```
(register! :pong (spawn pong))
(spawn pong)
(spawn ping)
; a new game begins !
```

## Watching over other actors lifecycles

现在 actors 可以从其他 actor 生命周期获得自动通知，例如，某个 actor 即将结束其生命周期。你可以使用 watch! 来接收消息：

```
(let [actor1 (spawn #(sleep 1000))
      actor2 (spawn
               (fn []
                 (let [w (watch! actor1)]
                   (receive
```

```
ch (channel)
fiber (spawn-fiber
    (fn []
      (let [m1 (rcv ch)
            m2 (rcv ch)]
        (+ m1 m2))))]
(sleep 200)
(snd ch 10)
(sleep 200)
(snd ch 10)
(join fiber))
; 20
```

通道可以在不同的 fibers 之间使用和共享：

```
(let [ch (channel 0 :block true true)
      f #(rcv ch)
      fiber1 (spawn-fiber f)
      fiber2 (spawn-fiber f)]
  (snd ch "m1")
  (snd ch "m2")
  (close! ch)
  #{(join fiber1) (join fiber2)})
; #{"m1" "m2"})
```

最后，通道可以用突发模式使用 rcv-into 和 snd-seq：

```
(let [ch (channel)
      fiber (spawn-fiber #(rcv-into [] ch 1000))]
  (snd-seq ch (range 200))
  (close! ch)
  (join fiber))
; [0 1 2 3 4 ...])
```

## Blazar

事情发展到你想知道如何将 fibers 和 socket 服务器链接起来，这里就是 Blazar，一个基于 fibers 构建的超快 HTTP 服务器框架，同时支持 WebSocket。该项目没有最近的提交，但与新版本的 Pulsar 配合得很好，其核心思想很巧妙。

project.clj 文件需要一点注意：

```
(defproject blazarland "0.1.0-SNAPSHOT"
  :java-agents [[co.paralleluniverse/quasar-core "0.7.5"]]
  :profiles {
    :dev {
      :resource-paths ["resources" "profiles/dev/resources"]
      :source-paths ["src" "profiles/dev/src"]}}
  :dependencies [[org.clojure/clojure "1.8.0"]
  [blazar "0.1.1"]
  [co.paralleluniverse/pulsar "0.7.5"]])
```

但随后你的基于 fiber 的服务器可以用几行代码编写！

```
(ns quasarland.blazar
  (:require
    [co.paralleluniverse.pulsar.core :as pc]
    [blazar.http.server :as bs]))
(pc/join
  (pc/spawn-fiber
    # (bs/start-fiber-ring-adapter
      "0.0.0.0"
      8080
      (fn [req] "Hello fiber world!"))))
```

为了本书的目的，这在标准 Raspberry Pi 上扩展得相当不错。

## Chapter 7. Advanced Tips

在本章中，我们将涵盖以下主题：

- Hacking the Clojure code
- Using Reader Conditionals, compile to Clojure, and ClojureScript
- Real-time shared development with an REPL
- Declarative data descriptions and validations with plumatic/schema

## Introduction

我们将在本章学习更高级的技巧。首先我们将 hack Clojure 代码以了解 Clojure 代码的工作原理。我们将从 Clojure 代码生成类文件然后反编译 Java 代码。理解 Clojure 代码很有趣且有用。

接下来，我们将看看如何编写在 Clojure 和 ClojureScript 之间保持兼容的源代码，并在两种环境中运行。随着 ClojureScript 如今越来越流行，单份代码在两种环境中运行可提高可维护性，尤其是在编写库时。

然后，我们将尝试从远程客户端实时共享 REPL。使用此机制，我们将尝试从你的 PC 控制 Raspberry Pi。

最后，我们将学习 prismatic schema，它使我们能够为数据定义和验证模式。此库非常有用，可用于定义数据形状和验证。我们将介绍 `core.types` 和 `clojure.spec`，它们是 Clojure 1.9 的最主要特性。

## Hacking the Clojure code

在你运行的大量代码中，如果你遇到过 Clojure 本身的 bug 会非常惊讶。

可能缺少一些特性，但核心令人印象深刻地健壮和稳定。

在幕后，Clojure 本身将字节码生成委托给一个名为 ASM 的库。ASM 生成 Java 机器可以执行的字节码。这在 Clojure 程序运行时实时完成，也可以在提前完成，例如当你想打包代码时。

这个非常简短的配方将查看如何编译 Clojure 代码并查看生成的 Java 字节码。

## Getting ready

Clojure 代码托管在 GitHub 上，因此要检索代码，请前往 https://github.com/clojure/clojure。执行 git checkout，请使用以下命令：

```
git clone https://github.com/clojure/clojure.git
```

要构建你自己的 Clojure 版本，你需要一个主要的 Java 构建工具，称为 maven，截至本文撰写时版本为 3。下载并将二进制文件 mvn 放在你的终端可访问的位置：

```
mvn package -Dmaven.test.skip=true
```

Clojure 令人惊讶地非常精简且编译速度很快。打包只需几秒钟。完成后，你可以在 target 文件夹中找到以下两个重要文件：

```
clojure-1.9.0-master-SNAPSHOT-slim.jar
```

## clojure-1.9.0-master-SNAPSHOT.jar

第一个 JAR 包含编译的 Java 类和未编译的 Clojure 代码，而第二个还包含编译的 Clojure 源代码。这使得 slim JAR 约为 1 MB，而普通 JAR 约为 3.5 MB。

拥有编译的 Clojure 文件的优点是启动虚拟机和 Clojure 环境时等待时间更少。

作为提醒，你可以像这样轻松启动 Clojure Server REPL：

```
java -cp \
target/clojure-1.9.0-master-SNAPSHOT-slim.jar \
-Dclojure.server.repl="{:port 5555 :accept clojure.main/repl}" \
clojure.main
```

或使用传统 REPL：

```
java -cp target/clojure-1.9.0-master-SNAPSHOT-slim.jar
clojure.main
```

我们将在几分钟后使用前面生成的 Clojure JAR，所以请记住它生成的位置。

现在来看生成的字节码。

## How to do it...

假设我们有一个非常简单的 Clojure 程序如下：

```
(ns gen3
  (:gen-class [:main true]))

(defn -main[]
  (println "hello world"))
```

前面的代码除了打印一些问候语外什么都不做，但它会使生成的代码更容易看到。

在解释这里使用的所有函数和单词之前，让我们先走一遍流程：

- 源文件需要在 Clojure 项目的通常源文件夹中，所以使用 Leiningen，这将是 `src/gen3.clj`
- 我们现在启动一个 REPL，再次使用 Leiningen，通过：

  `lein repl`
- 当 REPL 启动后，我们现在使用 compile 指令：

  ```
  (compile 'gen3)
  ; gen3
  ```

你会看到即使 REPL 很安静，类文件已被生成：

```
Nicolass-MacBook% ls -l target/classes
total 40
drwxr-xr-x@ 3 niko  staff    102 May 25 11:41 META-INF
-rw-r--r--@ 1 niko  staff    831 May 25 11:41 gen3$_main.class
-rw-r--r--@ 1 niko  staff   1302 May 25 11:41
gen3$fn__1248.class
-rw-r--r--@ 1 niko  staff   1494 May 25 11:41
gen3$loading__5569_auto___1246.class
-rw-r--r--@ 1 niko  staff   1788 May 25 11:41 gen3.class
-rw-r--r--@ 1 niko  staff   2372 May 25 11:41 gen3__init.class
```

那是生成了五个类，可能是最容易看到的是 `gen3.class` 文件。这是包含程序入口点的文件。让我们使用我们之前生成的 Clojure JAR 和新生成的类：

```
java -cp ../clojure/target/clojure-1.9.0-master-SNAPSHOT.jar:target/classes gen3
```

执行前面的命令将打印出预期的消息：

```
hello world
```

不错。所以我们现在可以看到 Clojure 代码正在被编译为字节

