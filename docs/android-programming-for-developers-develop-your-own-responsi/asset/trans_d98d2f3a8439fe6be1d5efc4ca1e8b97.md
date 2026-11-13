

理想情况下，我们当然不希望丢掉任何一帧。卡顿、无响应，尤其是 ANR，都会带来非常糟糕的用户体验，这会转化为差评和不受欢迎的应用。构建 Android 应用程序时应牢记的一条准则是：不要阻塞主线程！

## 注意

Android 在每台设备的开发者选项中提供了一个有用的严格模式设置，当应用程序在主线程上执行耗时操作时，它会在屏幕上闪烁。

在 Honeycomb（API 级别 11）中，平台引入了一个新的异常类 NetworkOnMainThreadException，它是 RuntimeException 的子类。如果系统检测到网络活动是在主线程上发起的，则会抛出该异常，从而提供了进一步的保护。

## 保持响应性

因此，理想情况下，我们可能希望将任何耗时操作从主线程转移出去，以便由后台的另一个线程处理，而主线程可以继续平滑处理用户界面更新，并及时响应用户交互。

应在后台线程处理的典型耗时任务包括以下内容：

- 网络通信
- 本地文件系统上的输入和输出文件操作
- 图像和视频处理
- 复杂的数学计算
- 文本处理
- 数据编码和解码

为了使此方法有用，我们必须能够协调工作，并在合作线程之间安全地传递数据——尤其是在后台线程和主线程之间，而异步编程正是为了解决这个问题而使用的。

让我们先从同步与异步图示入手：

![](img/35d01831c19688a44e50bce7c489007a_943_0.png)

前面的示例以图形方式展示了两种处理模型之间的主要区别。在左侧，数据下载任务发生在主线程上，占用该线程直到下载完成。因此，如果用户与 UI 交互并生成诸如触摸事件之类的事件，如果下载任务花费大量时间才能完成，应用程序将出现卡顿或变得无响应。

在右侧，异步模型会将下载数据任务交给另一个后台线程，使主线程可用于处理来自 UI 交互的任何事件。当下载的数据可用时，如果数据处理需要更新任何 UI 状态，后台任务可以将结果发布到主线程。

当我们使用异步模型来编程应用程序时，Android 操作系统还会利用最新设备上可用的额外 CPU 核心，同时执行多个后台线程，并提高应用程序的能效。

## 注意

这种同时执行的、可能相互交互的独立代码路径称为 **并发**。

为了完成一个工作单元而并行执行工作子单元称为 **并行**。

## Android 中的并发

如前所述，为了在多核设备环境中实现可扩展的应用程序，Android 开发者应该能够创建并发执行路径，以合并和聚合来自多个资源的数据。

Android SDK 基于源自 Apache Harmony 项目的 Java SDK 子集，提供对低级并发构造（如 java.lang.Thread、java.lang.Runnable 以及 synchronized 和 volatile 关键字）的访问。

这些构造是实现并发和并行的最基本构建块，所有高级异步构造都是围绕这些构建块创建的。

最基本的一个是 java.lang.Thread，它是最常用的类，也是在 Java 程序中创建新独立执行路径的构造：

```
public class MyThread extends Thread {
    public void run() {
        Log.d("Generic", "My Android Thread is running ...");
    }
}
```

在前面的代码中，我们子类化了 java.lang.Thread 来创建自己的独立执行路径。当 Thread 启动时，run 方法会自动被调用，并在 Android 日志中打印消息：

```
MyThread myThread = new MyThread();
myThread.start();
```

此时，我们将创建 MyThread 的实例，并在第二行启动它时，系统会在进程内创建一个线程并执行 run() 方法。

其他有用的线程相关方法包括以下内容：

- Thread.currentThread(): 检索当前正在运行的线程实例
- Thread.sleep(time): 在给定时间内暂停当前线程的执行
- Thread.getName() 和 Thread.getId(): 分别获取名称和 TID，以便用于调试目的
- Thread.isAlive(): 检查线程当前是否正在运行或是否已完成工作
- Thread.join(): 阻塞当前线程，并等待被访问的线程完成执行或终止

Runnable 接口是来自 Java API 的另一个构建块，是一个定义用于指定和封装旨在由 Java 线程实例或任何其他处理该 Runnable 的类执行的代码的接口：

```
package java.lang;

public interface Runnable {
    public abstract void run();
}
```

在下面的代码中，我们基本上创建了 Runnable 子类，使其实现 run() 方法，并可以被线程传递和执行：

```
public class MyRunnable implements Runnable {
    public void run(){
        Log.d("Generic", "Running in the Thread " +
        Thread.currentThread().getId());
        // Do your work here
        ...
    }
}
```

现在，我们的 Runnable 子类可以传递给 Thread，并在并发执行路径中独立执行：

```
Thread thread = new Thread(new MyRunnable());
thread.start();
```

虽然启动新线程很容易，但并发实际上是一件非常困难的事情。并发软件面临许多问题，分为两大类：正确性（产生一致且正确的结果）和活性（朝着完成取得进展）。线程创建也可能导致一些性能开销，线程过多会降低性能，因为操作系统必须在这些执行路径之间切换。

## 并发程序中的正确性问题

正确性问题的一个常见示例是，当两个线程需要根据当前值修改同一变量的值时。假设我们有一个名为 myInt 的整数变量，当前值为 2。

为了递增 myInt，我们首先需要读取其当前值，然后向其中加 1。在单线程世界中，两次递增将严格按顺序发生——我们将读取初始值 2，向其中加 1，将新值设回变量，然后重复该序列。两次递增后，myInt 的值为 4。

在多线程环境中，我们会遇到潜在的时序问题。尝试递增变量的两个线程可能会都读取相同的初始值 2，向其中加 1，并将结果（两种情况均为 3）设回变量：

```
int myInt = 2;
...
public class MyThread extends Thread {
    public void run() {
        super.run();
        myInt++;
    }
}
...
Thread t1 = new MyThread();
Thread t2 = new MyThread();
t1.start();
t2.start();
```

两个线程在其局部视图中都表现正确，但就整体程序而言，我们显然会遇到正确性问题；2 + 2 不应该等于 3！这种时序问题称为竞态条件。

解决竞态条件等正确性问题的常见方法是互斥——防止多个线程同时访问某些资源。通常，这是通过确保线程在读取或更新共享数据之前获取独占锁来实现的。

