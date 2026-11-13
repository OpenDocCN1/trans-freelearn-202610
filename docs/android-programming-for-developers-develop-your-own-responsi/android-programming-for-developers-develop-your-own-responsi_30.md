## 2. 模块 2
### 1. Android 异步编程
- Android 软件堆栈
- Dalvik 运行时
- ART 运行时
- 内存共享与 Zygote
- Android 进程模型
- 进程等级
- 进程沙箱
- Android 线程模型
- 主线程
- 应用无响应 (ANR) 对话框
- 保持响应性
- Android 中的并发
- 并发程序的正确性问题
- 并发程序的活性问题
- 线程协调
- 并发包构造
- Executor 框架
- Android 主要构建块
- Activity 并发问题
- 操作用户界面
- Service 并发问题

#### 启动服务的问题
#### 绑定服务的问题
#### 单独进程中的服务
#### 广播接收器并发问题
#### Android 并发构造
#### 总结

### 2. 使用 Looper、Handler 和 HandlerThread 执行工作
- 理解 Looper
- 理解 Handler
- 向 Looper 发送工作
- 使用 post 调度工作
- 使用 Handler 延迟工作
- 隐式引用泄漏
- 显式引用泄漏
- 使用 Handler 更新 UI
- 取消挂起的 Runnable
- 使用 send 调度工作
- 取消挂起的消息
- 组合与继承
- 使用 Handler 和 ThreadHandler 进行多线程
- Looper 消息分发调试
- 发送消息与发布 Runnable
- Handler 和 HandlerThread 的应用
- 总结

#### 总结

### 3. 探索 AsyncTask
- 介绍 AsyncTask
- 声明 AsyncTask 类型
- 执行 AsyncTasks
- 提供不确定进度反馈
- 提供确定进度反馈
- 取消 AsyncTask
- AsyncTask 执行状态
- 处理异常
- 控制并发级别
- 常见 AsyncTask 问题
- 碎片化问题
- 内存泄漏





#### Activity lifecycle issues
- 通过提前取消处理生命周期问题
- 通过保留的无界面片段处理生命周期问题

#### AsyncTask 的应用
- 摘要

### 4. 探索 JobScheduler API
- JobScheduler 简介
- 设置运行标准
- 调度作业
- 实现 JobService
- 列出待处理的作业
- 取消作业
- 调度周期性作业
- JobScheduler 的应用
- 摘要

### 5. 与网络交互
- 介绍 Android HTTP 客户端
- AndroidHttpClient
- HttpURLConnection
- 异步执行 HTTP 请求
- 检索文本响应
- 与 JSON Web API 交互
- 将 Java 对象转换为 JSON
- 与 XML Web API 交互
- 将 Java 对象转换为 XML
- 将 XML 转换为 Java 对象
- 自定义 HTTP 超时
- 通过 SSL 会话安全通信
- 摘要

### 6. 与 GCM 的网络交互
- GCM 简介
- 为您的应用程序设置和配置 GCM
- 注册 GCM 接收器
- 设置注册服务
- InstanceID 监听器
- 接收下行消息
- 接收来自主题的消息

#### 发送上行消息
#### GcmListenerService 传递回调
#### 使用 GCM Network Manager 执行任务
- 构建一次性任务
#### 摘要

### 7. 使用 RxJava 进行异步编程
- RxJava 简介
- Cold 与 Hot Observable
- RxJava 设置
- 创建 Observables
- 转换 Observables
- 理解 Schedulers
- 使用 Schedulers 执行 IO 操作
- 取消订阅
- 组合 Observables
- 监视事件流
- 合并 Observables
- 使用 RxJava 观察 UI 事件
- 使用 Subjects
- 摘要
