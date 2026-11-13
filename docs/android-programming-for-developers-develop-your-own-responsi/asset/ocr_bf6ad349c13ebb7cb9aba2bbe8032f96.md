+   - Executors.newCachedThreadPool(): This is a thread poll that could grow and reuse previously created threads
+   - Executors.newFixedThreadPool (nThreads): This is a thread pool with a fixed number of threads and a message queue for store work
+   - Executors.newSingleThreadPool(): This is similar to newFixedThreadPool, but with only one working thread

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
        executor.execute(new MyRunnable()));
    }
}
```

In the preceding code, we created ThreadPool over the factory methods with a fixed number of five threads ready to process work.

After the ExecutorService instance creation, new Runnable tasks are posted for asynchronous processing.

When a new unit of work is submitted, a thread that is free to work is chosen to handle the task; but when all the threads are occupied, Runnable will wait in a local queue until a thread is ready to work.