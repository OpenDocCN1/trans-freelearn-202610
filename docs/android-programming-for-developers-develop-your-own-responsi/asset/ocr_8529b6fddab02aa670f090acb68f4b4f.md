# # Understanding Schedulers

There is an incorrect misconception and belief that RxJava processing is multithreaded by default. An observable and the list of transformations applied by the specified operators occur on the same thread that the subscription is made.

Hence, on Android, if the subscription is carried out on the main thread, the operators chain processing will run on the main thread, blocking the UI until the work is done.

While this behavior might work for lightweight processing tasks, when the operation requires IO interaction or CPU-intensive computing, the task execution might block the main Thread and crash the application with an ANR.

To simplify the asynchronous and concurrent executions, the RxJava framework allows us to define a Scheduler entity that defines the thread where a unit of work is executed.

The subscribeOn(Scheduler) operator allows us to set the Scheduler that defines the thread on which the subscription has been made and the Observable will start to operate.

When no Scheduler is specified, the Observable and operations will run on the thread that invoked the subscribe function.

On Android, a subscribe function is typically invoked from an Android Activity or Fragment that runs on the main Thread, then if any operation takes a substantial amount of time to finish it will block the UI Thread and degrade the UI responsiveness.

By controlling the thread where the subscription is made, we are controlling the thread where the Observable and its operators are going to execute and even the thread where the subscriber will receive the callbacks.

The observeOn(Scheduler) allows us to set the Scheduler that defines the thread in which the Observer callbacks (onNext, onError, onCompleted) are invoked.