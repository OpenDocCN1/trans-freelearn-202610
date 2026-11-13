

## Concurrent package constructs

Other Java concurrent constructs provided by java.util.concurrent, which are also available on Android SDK are as follows:

- **锁对象** (java.util.concurrent): 它们使用更高级别的惯用法实现锁定行为。
- **执行程序**: 这些是用于启动和管理一组线程执行（线程池等）的高级别 API。
- **并发集合**: 这些是修改集合的方法受到保护以避免同步问题的集合。
- **同步器**: 这些是用于协调和控制线程执行（信号量、循环栅栏等）的高级别构造。
- **原子变量** (java.util.concurrent.atomic): 这些是提供对单个变量进行线程安全操作的类。其中一个示例是 AtomicInteger，可在我们的示例中用于解决正确性问题。

Some Android-specific constructs use these classes as basic building blocks to implement their concurrent behavior, although they could be used by a developer to build custom concurrent constructs to solve a specific use case.

## Executor framework

The Executor framework is another framework available on java.util.concurrent that provides an interface to submit Runnable tasks, decoupling the task submission from the way the task will run:

```
public interface Executor {
 void execute(Runnable command);
}
```

Each Executor, which implements the interface that we defined earlier, can manage the asynchronous resources, such as thread creation destruction and caching, and task queueing in a variety of ways to achieve the perfect behavior to a specific use case.

The java.util.concurrent comes with a group of implementations available out of the box that cover most generic use cases, as follows:

- Executors.newCachedThreadPool(): 这是一个可以增长并重用先前创建的线程的线程池
- Executors.newFixedThreadPool(nThreads): 这是一个具有固定数量线程和用于存储工作的消息队列的线程池
- Executors.newSingleThreadPool(): 这与 newFixedThreadPool 类似，但只有一个工作线程

To run a task on Executor, the developer has to invoke execute() by passing Runnable as an argument:

```
public class MyRunnable implements Runnable {
    public void run() {
        Log.d("Generic", "Running From Thread " +
        Thread.currentThread().getId());
        // Your Long Running Computation Task
    }
}
public void startWorking(){
    Executor executor = Executors.newFixedThreadPool(5);
    for ( int i=0; i < 20; i++ ) {
        executor.execute(new MyRunnable());
    }
}
```

In the preceding code, we created ThreadPool over the factory methods with a fixed number of five threads ready to process work.

After the ExecutorService instance creation, new Runnable tasks are posted for asynchronous processing.

When a new unit of work is submitted, a thread that is free to work is chosen to handle the task; but when all the threads are occupied, Runnable will wait in a local queue until a thread is ready to work.

## Android primary building blocks

A typical Android application is composed of the following four main building blocks:

- android.app.Activity
- android.app.Service
- android.content.BroadcastReceiver
- android.content.ContentProvider

The Activity, Service, and BroadcastReceiver are activated explicitly or implicitly over an asynchronous message called Intent.

Each of these building blocks have their own life cycle, so they could be exposed to different concurrency issues if an asynchronous architecture is used to offload work from the main thread.

## Activity concurrent issues

The Activity building block has a tight connection with a presentation layer because it's the entity that manages the UI view over a defined tree of fragments and views that display information and respond to user interactions.

Android applications are typically composed of one or more subclasses of android.app.Activity. An Activity instance has a very well-defined lifecycle that the system manages through the execution of lifecycle method callbacks, all of which are executed on the main thread.

To keep the application responsive and reactive, and the activity transition smooth, the developer should understand the nature of each Activity lifecycle callback.

The most important callbacks on the Activity lifecycle are as follows:

- onCreate(): 此时 Activity 不可见，但所有私有 Activity 资源（视图和数据）都是在这里创建的。长时间和密集的计算应异步完成，以减少用户在 Activity 转换期间无法获得可视反馈的时间。
- onStart(): 当 UI 可见但无法在屏幕上交互时调用的回调。此阶段的任何滞后都可能让用户感到不悦，因为此时生成的任何触摸事件都会被系统错过。
- onResume(): 当 Activity 即将进入前台并处于可交互状态时调用的回调。
- onPause(): 当 Activity 即将进入后台且不可见时调用的回调。计算应尽快结束，因为下一个 Activity 不会在该方法结束之前恢复。
- onStop(): 当 Activity 不再可见但可以重新启动时调用的回调。
- onDestroy(): 当 Activity 实例即将在后台销毁时调用的回调。属于该实例的所有资源和引用都必须释放。

An Activity instance that is completed should be eligible for garbage collection, but background threads that refer to Activity or part of its view hierarchy can prevent garbage collection and create a memory leak.

Similarly, it is easy to waste CPU cycles (and battery life) by continuing to do background work when the result can never be displayed as Activity is completed.

Finally, the Android platform is free at any time to kill processes that are not the user's current focus. This means that if we have long-running operations to complete, we need some way of letting the system know not to kill our process yet.

All of this complicates the do-not-block-the-main-thread rule as we need to worry about canceling background work in a timely fashion or decoupling it from the Activity lifecycle where appropriate.

## Manipulating the user interface

The other Android-specific problem lies not in what you can do with the UI thread, but in what you cannot do.

### Note

You cannot manipulate the user interface from any thread other than the main thread.

This is because the user interface toolkit is not thread-safe, that is, accessing it from multiple threads may cause correctness problems. In fact, the user interface toolkit protects itself from potential problems by actively denying access to user interface components from threads other than the one that originally created these components.

If the system detects this, it will instantly notify the application by throwing CalledFromWrongThreadException.

The final challenge then lies in safely synchronizing background threads with the main thread so that the main thread can update the user interface with the results of the background work.

If the developer has access to an Activity instance, the runOnUiThread instance method can be used to update the UI from a background thread.

The method accepts a Runnable object like the one used to create an execution task for a thread:

```
public final void runOnUiThread (Runnable)
```

In the following example, we are going to use this facility to publish the result from a synonym search that was processed by a background thread.

To accomplish the goal during the onCreate activity callback, we will set up onClickListener to run searchTask on a created thread:

```
// Get the Views references
Button search = (Button) findViewById(R.id.searchBut);
final EditText word = (EditText) findViewById(R.id.wordEt);
```

