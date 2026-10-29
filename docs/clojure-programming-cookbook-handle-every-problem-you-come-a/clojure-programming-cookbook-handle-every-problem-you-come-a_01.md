前言

本书涵盖内容

阅读本书所需条件

本书适合人群

章节

准备工作

如何操作...

其工作原理...

更多内容...

另请参阅

约定

读者反馈

客户支持

下载示例代码

下载本书彩色图片

勘误

盗版

问题

1. 使用 Clojure 进行实时编程

介绍

启动 REPL！

准备工作

如何操作...

其工作原理...

更多内容...

另请参阅

使用原始数据类型

准备工作

如何操作...

使用数字

使用字符串和字符

使用布尔值和 nil

使用符号和关键字

其工作原理...

更多内容...

使用 var 绑定、条件、循环和错误处理

准备工作

如何操作...

def 和 let

if 和 if-not

when 和 when-not

case 和 cond

do 和 dotimes

loop 和 recur

try... catch... throw

其工作原理...

使用和定义函数

准备工作

如何操作...

定义简单函数

定义可变参数函数

定义多重arity函数

定义使用关键字指定参数的函数

定义带有前置条件和后置条件的函数

其工作原理...

更多内容...

使用第三方库

准备工作

如何操作...

手动将 JAR 文件添加到类路径

使用 Leiningen 和 project.clj 文件

查看依赖项
一次性
在运行时添加新依赖项
使用命名空间
准备工作
如何操作...
创建新的命名空间
检查命名空间
使用 :import 和 :refer 添加函数
从文件加载命名空间
重新加载命名空间
如何组织命名空间
更多内容...
公共 API 的命名空间
tools.namespace
下一步？
2. 与集合交互
介绍
Clojure 集合及其基本函数
准备工作
如何操作...
列表
向量
映射
集合
其工作原理...
Clojure 集合
列表和向量之间的差异
Clojure 是不可变的
更多内容...
在不同类型之间转换集合
使用 distinct
访问和更新集合中的元素
准备工作
如何操作...
使用 nth 函数访问集合

使用 get 访问映射或集合

映射、集合和关键字是访问集合的函数

使用 second、next、ffirst 和 nfirst 访问集合

使用 update 更新集合

其工作原理...

更多内容...

为向量使用 get

使用集合作为映射中的键

使用 get-in

使用 assoc-in

使用 update-in

从集合中过滤元素

准备工作

如何操作...

过滤三的倍数

过滤映射的项目

过滤非 nil 值

从序列中移除奇数值

使用 keep

使用 keep-indexed

更多内容...

Filter 作为转换器

使用 core.async 进行过滤

转换和排序集合

准备工作

如何操作...

使用内置 sort

迁移到 sort-by

使用 sorted-set

处理排序集合子集

排序 Clojure 映射

排序映射结构

你排序好了吗？

其工作原理...

更多内容...

排序文件行

合并和拆分集合

准备工作

如何操作...

使用 merge 和 merge-with 进行合并

zipmap 合并映射的键和值

使用 interleave

使用 split-at 和 split-with 拆分集合

其工作原理...

更多内容...

使用 group-by 拆分集合

使用 filter 函数拆分集合

如何变得懒惰

准备工作

如何操作...

iterate 函数

已实现？

使用 repeatedly 生成随机序列

使用 macro lazy-seq

其工作原理...

更多内容...

其他产生 lazy-seq 的函数

使用 doall 强制求值

使用文件的惰性求值

map 函数和调用 rest

3. Clojure Next

介绍

使用解构技术

准备工作

如何操作...

顺序解构

使用映射解构

其工作原理...

更多内容...

字符串的顺序解构

使用 core.match 进行条件解构

使用函数式编程风格

准备工作

如何操作...

函数将函数作为其参数

map

reduce

apply

使用匿名函数

使用 fn 的匿名函数

使用函数字面量定义匿名函数

返回函数的函数

constantly

complement

partial

comp

every-pred 和 some-fn

其工作原理...

Clojure 函数是第一类函数

纯函数

更多内容...

reduce 和 apply

loop 和 recur

使用线程宏

准备工作

如何操作...

介绍 -> 宏

介绍 ->> 宏

介绍 cond-> 和 cond->> 宏

介绍 some-> 和 some->>

其工作原理...

更多内容...

介绍 as->

Flambo 预览

定义简单宏

准备工作

如何操作...

你的第一个宏

你的第二个宏
其工作原理...
更多内容...
使用引号
无处不在的宏
定义更高级的宏
准备工作
如何操作...
跟踪代码何时开始
使用宏记录消息
其工作原理...
更多内容...
def test
使用协议
准备工作
如何操作...
定义记录
定义类型
定义协议
其工作原理...
记录和类型是 Java 类
区分记录和类型
更多内容...
定义可变协议和类型
定义多方法
准备工作
如何操作...
定义多方法
其工作原理...
更多内容...
为初学者讲解转换器
准备工作
如何操作...
其工作原理...
可组合函数
插入元素

- 复制文件
- 删除文件和目录
- 列出目录中的文件和目录
- 其工作原理...
  - slurp 和 spit
  - with-open 宏
- 更多内容...
  - 读取 HTTP 资源
  - 检查源代码
- 操作各种格式的文件 - XML、JSON、YAML 和 EDN
  - 准备工作
  - 如何操作...
    - XML
    - JSON
    - YAML 不是标记语言
    - MessagePack，Clojure 库
  - 其工作原理...
  - 更多内容...
    - 可扩展数据表示法
- 使用 Clojure HTTP 客户端
  - 准备工作
  - 如何操作...
  - 其工作原理...
  - 更多内容...
    - 异步 HTTP 和 core async
- 在 RabbitMQ 中使用队列和主题
  - 准备工作
    - 安装 RabbitMQ
  - 如何操作...
    - 生产和消费 hello world 消息
      - 声明命名空间并加载 Langoh r 库和 Docker 容器的 IP 地址
      - 定义生产者函数
      - 定义消息处理器和消费者
      - 测试生产者和消费者

使用阻塞消费者

其工作原理...

连接到代理并打开通道

生产者如何工作

声明队列

生产消息

消费者如何工作

消费者消息

定义消息处理器

更多内容...

使用扇出交换机

fanout-producer

fanout-consumer

运行扇出消费者

测试扇出消费者

使用主题交换机

Topic-publisher

Topic-consumer

运行主题消费者

测试主题消费者

另请参阅

使用 Kafka

准备工作

下载 Kafka 0.8.2.2

启动 ZooKeeper 和 Kafka 服务器

创建主题

运行控制台消费者

使用 clj-kafka

如何操作...

声明命名空间并加载 Clojure 库

生产消息

消费消息

其工作原理...

生产消息

消费消息

更多内容...





- 访问实例字段
- 访问类字段
- 访问内部类
- 引用类
- 使用数组
- 如何工作...
- Clojure 访问 Java 方法和对象的总结
- Clojure 不继承 Java 的受检异常
- 数组访问的总结
- 打印 Java 原语的字符串
- 还有更多...
- 使用双点宏进行链式调用
- 使用 doto 宏
- 使用反射
- 扩展 Java 超类并实现 Java 接口
- 准备工作
- 如何操作...
- 使用 proxy
- 使用 reify 实现接口
- 如何工作...
- 还有更多...
- 在 Clojure 中定义匿名类
- 使用注解
- 从 Java 调用 Clojure
- 准备工作
- 如何操作...
- 使用 gen-class 定义一个简单的命名类
- 在 Clojure 中定义带实例方法和构造函数的类
- 创建可从 Java 调用的 JAR 文件
- 如何工作...
- 用于生成 Java 类的 project.clj
- AOT 编译
- 还有更多...
- 定义可从 Java 调用的接口和实现类

## 使用 Maven 进行 Clojure 和 Java 项目

## 从 Clojure 调用 Scala

## 准备工作

## 如何操作...

## 使用 Leiningen 开发 Scala 和 Clojure

## 导入 Scala 类

## 调用实例方法

## 调用单例方法

## 访问元组

## 访问 Scala 字段

## 如何工作...

## 还有更多...

## 使用 SBT

## 创建基于 SBT 的项目

## 编写源代码

## ClojureCLR

## 准备工作

## 安装 Visual Code Studio 并创建项目

## 安装 Mono Studio IDE

## Lein CLR

## 如何操作...

## 从 Clojure 调用 .NET 代码

## 启动 REPL

## 处理 IO

## 使用 C# 代码解析一些 XML

## 使用 Clojure/C# 调用 REST API

## UI 原型设计

## 把所有东西整合起来

## 如何工作...

## 编译 Clojure 代码供 .NET 使用

## 将 Clojure 代码添加到 Visual Studio 项目中

## 使用 Clojure.RT 运行 Clojure 代码

## 使用 Xamarin Mono Studio

## 还有更多...

## ClojureScript

## 准备工作

## 如何操作...

## 自动编译你的代码

## Fibonacci'ed

## 如何工作...

## 与 JavaScript 交互

## 从 ClojureScript 使用 jQuery

## 创建一个 ClojureScript 库

## 使用第三方库

## 还有更多...

## 在 REPL 中运行

## 编译同时适用于 Clojure 和 ClojureScript 的代码
