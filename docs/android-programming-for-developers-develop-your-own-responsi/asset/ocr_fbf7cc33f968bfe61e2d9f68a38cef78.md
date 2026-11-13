```
.subscribeOn(AndroidSchedulers.mainThread())
.subscribe(new MySubscriber());
```

As you know, to make changes in the Android UI, it is imperative to run your code on the main Thread. Hence, in order to receive doOnNext from the first Observable in the main thread, we invoke subscribeOn() with AndroidSchedulers.mainThread() forcing the first Observable, the one created with the just operator, to emit notifications to doOnNext in the main Thread.

As soon as doOnNext() receives the notification with the String carrying the URL to retrieve the text, we display ProgressDialog in the UI and we log a message in the Android Log.

Next, since we want to perform network operations off the main thread, using the observeOn operator, we force following observables to send notifications to the threads managed by the IO Scheduler. This means that the following operators and observables will execute and emit events in the IO Scheduler threads.

In the meantime, between each network operation, we intercept the start of the second network operation to print the message in Android with a doOnNext between getTextFromNetwork and translateOnNetwork Observables.

When the network operations finish, and before we update the UI with the results and we dismiss the progress dialog, we switch the execution to the main thread by again invoking the operator observeOn() with the main Thread Scheduler.

Before we display the results on the screen, with the doOnTerminate operator we register an Action function to be called to dismiss the progress dialog previously started. As described before, the function will be invoked, whether the chain terminates with success or with an error.

At the end, the subscriber callbacks will be invoked to update the UI with the results returned or to show an error message.

If the network operation terminates with success you should see a similar logging stream in the Android Log:

```
...54.390 I Network IO Operation will start T[main]
```