# # Monitoring the event stream

Although so far we have been using the observable operators to manipulate stream events, there are operators that allow us to monitor the events without changing them. These operators, known sometimes as utility operators, are able to react to the events or errors emitted on the observable chain created between the source observable and the final Subscriber without creating any side effects.

Let's enumerate them and explain the more common utility operators used to observe the event stream:

+   • doOnSubscribe(Action0): Registers an Action0 function to get called when a Subscriber subscribes to the Observable.
+   • doOnUnsubscribe(Action0): Registers an Action0 function to get called when a Subscriber unsubscribes from the Observable.
+   • doOnNext(Action1): Registers an Action1 to be called when a new event is emitted from the source Observable. The Event <T> object is also passed as an argument to the Action1 function.
+   • doOnCompleted(Action0): Registers an Action0 function to be called when the source Observable emits the onComplete event.
+   • doOnError(Action1): Registers an Action1 function to be called when an error is emitted from the source Observable. The Throwable emitted on the OnError is also passed to the Action1.call function.
+   • doOnTerminate(Action0): Registers an Action0 function to be invoked when an error or onComplete is emitted by the source Observable. This callback function also means that the previous Observable will emit no more items.

These multipurpose operators will allow us to observe and debug complex chains that usually involve several transformations, create progress dialogs to show progress, cache results, and even generate processing analytics.

In our next example, we will make use of these operators to log the progress of our previous multi network operation in the Android Log, and to present a progress dialog on screen as long as the operation is progressing:

```
Observable.just(RETRIEVE_TEXT_URL)
```