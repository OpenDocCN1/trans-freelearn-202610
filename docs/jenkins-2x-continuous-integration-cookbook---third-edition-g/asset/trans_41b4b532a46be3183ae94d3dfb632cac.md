

## 准备工作

您需要拥有 Jenkins 的管理员访问权限。

## 如何操作...

1. 进入 Jenkins 仪表盘，然后 管理 Jenkins 准备关机：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_166_0.png)

2. 再次进入 Jenkins 仪表盘，然后 管理 Jenkins 以 取消关机：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_166_1.png)

[取消关机](https://)

## 使用 JavaMelody 监控 Jenkins

JavaMelody (http://code.google.com/p/javamelody/) 是一个开源项目，提供全面的监控。Jenkins 插件同时监控 Jenkins 的主实例及其节点。插件提供大量详细的重要信息。您可以查看从 1 天或 1 周到数月的演变图表，显示主要指标，如 CPU 或内存。演变图表非常擅长定位资源消耗大的计划作业。

`Monitoring` 插件提供使用 JavaMelody 对 Jenkins 的监控。它提供 CPU、内存、系统负载平均值、HTTP 响应时间等的图表。它还提供 HTTP 会话、错误和日志、垃圾回收操作、堆转储、无效会话等详细信息。

## 准备工作

从 Jenkins 仪表盘安装 Monitoring 插件：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_168_0.png)

## 如何操作...

1. 在 Jenkins 仪表盘上，单击 管理 Jenkins。单击 Jenkins 主机的监控，如以下屏幕截图所示：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_169_0.png)

2. 它将打开 JavaMelody 监控的统计信息，如以下屏幕截图所示。观察所有统计信息：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_169_1.png)

3. 单击 其他图表 并观察与垃圾回收时间相关的其他详细信息，

## 线程

LAPTOP-FQ8JSR2E@192.168.99.1 上的线程：数量 = 42，最大 = 51，已启动总数 = 789 详细信息

| 线程 | 守护进程？ | 优先级 | 状态 | 执行的方法 | Cpu 时间 (ms) | 用户时间 (ms) | Kill |
|---|---|---|---|---|---|---|---|
| Attach Listener | yes | 5 | RUNNABLE |  | 0 | 0 |  |
| AWT-Window | yes | 6 | RUNNABLE | sun.awt.windows.WToolkit.eventLoop(Native Method) | 93 | 31 |  |
| DestroyJavaVM | no | 5 | RUNNABLE |  | 3,515 | 2,203 |  |
| FilePath localPool [6d] | yes | 5 | TIMED_WAITING | sun.misc.Unsafe.park(Native Method) | 15 | 0 |  |
| Finalizer | yes | 8 | WAITING | java.lang.Object.wait(Native Method) | 328 | 109 |  |
| Handling GET monitoring from 0.0.0.0:0.1 : RequestHandlerThread[0x309] | yes | 5 | RUNNABLE | java.lang.Thread.sleep(Native Method) | 1,546 | 1,187 |  |
| [CHub]1 Selector (sun.nio.ch. gen 0) : Computer threadPoolForRemoting [9] | yes | 5 | RUNNABLE | sun.java2d.Windows.Selector$SubSelector.poll(Native Method) | 0 | 0 |  |
| Java2D Disposer | yes | 10 | WAITING | java.lang.Object.wait(Native Method) | 15 | 0 |  |
| javanotify | yes | 5 | TIMED_WAITING | java.lang.Object.wait(Native Method) | 140 | 15 |  |
| Jenkins cron thread | no | 5 | WAITING | java.lang.Object.wait(Native Method) | 0 | 0 |  |
| Jenkins UDP 33849 monitoring thread | no | 5 | RUNNABLE | sun.net.www.http.HttpClient.parseHeaders(Native Method) | 0 | 0 |  |
| jenkins.util.Timer [#10] | yes | 5 | TIMED_WAITING | sun.misc.Unsafe.park(Native Method) | 500 | 93 |  |
| jenkins.util.Timer [#1] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 359 | 125 |  |
| jenkins.util.Timer [#2] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 328 | 109 |  |
| jenkins.util.Timer [#3] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 578 | 218 |  |
| jenkins.util.Timer [#4] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 484 | 156 |  |
| jenkins.util.Timer [#5] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 328 | 156 |  |
| jenkins.util.Timer [#6] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 540 | 171 |  |
| jenkins.util.Timer [#7] | yes | 5 | WAITING | sun.misc.Unsafe.park(Native Method) | 593 | 205 |  |

Java Melody - 线程

## 更多内容...

监控是全面测试和故障排除的基础。本节探讨这些问题之间的关系以及插件中公开的度量。

## 使用 JavaMelody 故障排除 - 内存

您的 Jenkins 服务器有时会因贪婪的构建、泄漏的插件或基础设施中的某些隐藏复杂性而出现内存问题。JavaMelody 提供了全面的内存度量范围，包括堆转储和内存直方图。

Java 虚拟机将内存划分为各个区域，并通过删除没有引用其他对象的对象来清理。垃圾回收在繁忙时可能占用大量 CPU，并且越接近内存耗尽，垃圾回收就越繁忙。对外部监控代理来说，这看起来像是难以追踪的 CPU 峰值。仅仅因为垃圾收集器管理内存，并不意味着 Java 中不存在内存泄漏的可能性。内存可能因自定义缓存或调用本地库等常见做法而被保留过久。

缓慢的内存泄漏会在内存相关演变图上显示为缓慢的斜坡。如果您怀疑存在内存泄漏，则可以让插件通过 执行垃圾收集器 链接强制进行完全垃圾回收。如果不是内存泄漏，缓慢的斜坡会突然下降。

内存问题也可能表现为垃圾收集器拼命清理但几乎清理不出足够空间时的大 CPU 峰值。垃圾收集器在全面查找不再引用的对象时也会暂停应用程序（“全局停顿垃圾收集”），从而导致 Web 浏览器请求的响应时间变长。这可以通过 统计 http - 1 天 中的平均值和最大值看到。

## 使用 JavaMelody 故障排除 - 痛苦的作业

您应考虑以下几点：

- **卸载工作：** 为了获得稳定的基础设施，尽可能将尽可能多的工作从主实例卸载。如果您有计划任务，请在时间上将最重的任务分开。时间分离不仅使负载更均匀，而且还通过观察 JavaMelody 的演变图表使找到有问题的构建变得更容易。还应考虑空间分离；如果某个节点或一组标记的节点显示出问题，则开始在机器位置之间切换作业，并通过 http://host:port/monitoring/nodes 查看其各自的性能特征。
- **硬件很便宜：** 与支付人工时间相比，购买额外的 8 GB 大约相当于一个人工小时的成本。

一个常见的陷阱是向服务器添加内存，同时忘记更新初始化脚本以允许 Jenkins 使用更多内存。

- **审查构建脚本：** Javadoc 生成和自定义 Ant 脚本可以分叉 JVM 并保留在其自身配置中定义的内存。编程错误也可能是沮丧的原因。不要忘记查看 JavaMelody 关于 统计系统错误日志 和 统计 http 系统错误 的报告。
- **不要忘记外部因素：** 因素包括备份、cron 作业、更新 locate 数据库和网络维护。这些将在演变图表中显示为周期性模式。
- **数量的力量：** 将 JavaMelody 与磁盘使用插件等结合使用，以保持对重要统计数据的全面概述。每个插件都易于配置，但它们对您的用处将比添加额外插件的维护成本增长得更快。

## 使用构建监控视图监控 Jenkins 作业

`Build Monitor` 插件提供所选 Jenkins 作业的状态和进度的可视化。它使用 AJAX 每隔几秒自动显示更新的视图。它也可以轻松适应不同的计算机屏幕尺寸。

