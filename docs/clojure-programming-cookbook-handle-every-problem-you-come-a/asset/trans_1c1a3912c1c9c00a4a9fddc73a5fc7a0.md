

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

