# Concurrent package constructs

Other Java concurrent constructs provided by java.util.concurrent, which are also available on Android SDK are as follows:

+   • **Lock objects** (java.util.concurrent): They implement locking behaviors with a higher level idiom.
+   • **Executors**: These are high-level APIs to launch and manage a group of thread executions (ThreadPool, and so on).
+   • **Concurrent collections**: These are the collections where the methods that change the collection are protected from synchronization issues.
+   • **Synchronizers**: These are high-level constructs that coordinate and control thread execution (Semaphore, Cyclic Barrier, and so on).
+   • **Atomic variables** (java.util.concurrent.atomic): These are classes that provide thread-safe operations on single variables. One example of it is AtomicInteger that could be used in our example to solve the correctness issue.

Some Android-specific constructs use these classes as basic building blocks to implement their concurrent behavior, although they could be used by a developer to build custom concurrent constructs to solve a specific use case.

# Executor framework

The Executor framework is another framework available on java.util.concurrent that provides an interface to submit Runnable tasks, decoupling the task submission from the way the task will run:

```
public interface Executor {
 void execute(Runnable command);
}
```

Each Executor, which implements the interface that we defined earlier, can manage the asynchronous resources, such as thread creation destruction and caching, and task queueing in a variety of ways to achieve the perfect behavior to a specific use case.

The java.util.concurrent comes with a group of implementations available out of the box that cover most generic use cases, as follows: