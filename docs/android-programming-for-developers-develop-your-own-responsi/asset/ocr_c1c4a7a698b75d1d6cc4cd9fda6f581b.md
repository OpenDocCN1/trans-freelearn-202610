# Performing IO operations with Schedulers

In the next example, we will use Schedulers to mirror the behavior of AsyncTask and retrieve text from the network on the background thread. Subsequently, the result will be published to a Subscriber that runs on the main Thread.

First of all, we will create a function that creates an Observable that emits the String retrieved from the network:

```
Observable<String> getTextFromNetwork(final String url) {
return Observable.create(
new Observable.OnSubscribe<String>() {
@Override
public void call(Subscriber<? super String> sub) {
try {
String text = downloadText(url);
sub.onNext(text);
sub.onCompleted();
} catch (Throwable t) {
sub.onError(t);
}
}
}
);
}
```

Before we specify the Scheduler used to run our asynchronous call, we need to state two assumptions:

+   - Since the code that runs on observable performs a network operation we must run Observable on the background thread
+   - To publish the result and update the UI, we must execute our Subscriber callbacks on the main Thread

Now, let's build up the asynchronous RxJava execution that retrieves the text and update the UI following the previous assumptions and using the Scheduler