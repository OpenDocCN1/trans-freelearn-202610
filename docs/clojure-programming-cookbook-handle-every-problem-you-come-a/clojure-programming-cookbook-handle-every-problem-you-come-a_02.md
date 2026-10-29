# 6. 并发与并行

## 引言

## 使用 Clojure 解决并发问题

## 准备工作

## 如何操作...

## 使用 atoms

## 创建和引用 atom

## 更新 atom

## 使用验证器

## 使用 CAS 操作

## 使用 ref 和 dosync 的软件事务内存

## 创建 ref

## 使用 alter 更新 refs

## 使用 ensure

## 使用 watcher 和改进的事务代码

## 使用 commute

## 使用 agents

## 创建 agents

## 更新 agents

## 如何工作...

## Clojure 中三种引用类型的总结

## 协调/非协调

## 同步/异步

## Clojure 中 STM 的工作原理

## Alter 和 commute

还有更多...

promise 和 deliver

pmap、pcalls 和 pvalues

pcalls

pvalues

另请参阅

## 使用 Akka 进行基于分布式参与者的开发

准备工作

如何操作...

你的第一个 okku

创建参与者

分派消息

如何工作...

参与者系统

理解参与者

还有更多...

调用远程参与者

请求和回复

另请参阅

## 使用 Spyglass 和 Couchbase 在 JVM 之间共享状态

准备工作

在 project.clj 中设置依赖项

将 Couchbase Server 设置为 Memcached 服务器

使用 docker-compose 启动 Couchbase Server

为 docker-compose 创建目录和文件

启动 Couchbase 集群

检查 Docker 进程是否正在运行

Couchbase 集群总结

使用 Couchbase Web 管理员控制台

登录 Web 控制台

向集群添加节点

添加集群

定义 Memcached 桶

如何操作...

定义使用 Spyglass 库

连接服务器

设置值

从另一个 REPL 获取值

测试生存时间

断开连接

如何工作...

什么是 Memcached？

从 Telnet 访问 Memcached

Couchbase Server

还有更多...

使用 get-multi

使用 async-get

使用 CAS 操作

另请参阅

使用 meltdown 进行响应式编程

准备工作

如何操作...

定义一个 reactor

向 reactor 发送消息

走同步方式

如何工作...

选择器

流

流和 reduce

使用图组合函数

自定义流

还有更多...

桥接 core.async

准备工作

记忆 - 回顾 core.async 基础

发布/订阅

如何操作...

异步套接字服务器

异步套接字客户端

计时

还有更多...

客户端到 Python WebSockets

## Clojure WebSocket 服务器

## 关于 Quasar/Pulsar

## 准备工作

## 如何操作...

## 如何工作...

## 监视其他参与者生命周期

## 参与者的状态

## 还有更多...

## Blazar
