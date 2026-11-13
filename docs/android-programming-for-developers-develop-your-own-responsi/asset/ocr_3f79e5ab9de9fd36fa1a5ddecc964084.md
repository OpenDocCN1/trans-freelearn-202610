To print the exact time when the subscription and unsubscription is made, we provided an Action0 function to the doOnUnsubscribe and doOnSubscribe that prints a message to the Android Log.

As a result, the code above should output the following output:

```
... 43.230 I Observer subscribed to PublishSubject
... 43.230 I New Event received from PublishSubject: 3
... 43.230 I New Event received from PublishSubject: 4
... 43.230 I Observer unsubscribed to PublishSubject
```

As described before, only the events dispatched while the final observer is subscribed are emitted to the Action callback. Therefore, the events submitted before the subscriptions and after the unsubscription are not received by our Subscriber.

Now, for comparison, let's try to compare the event stream emitted by a ReplaySubject, with the exact sequence of events submitted to the Subject.

Again, the ReplaySubject class was built by calling the create static function, and as a result, you should see the following output:

```
.600 I Observer subscribed to ReplaySubject
.600 I New Event received from ReplaySubject: 1
.600 I New Event received from ReplaySubject: 2
.600 I New Event received from ReplaySubject: 3
.600 I New Event received from ReplaySubject: 4
.600 I Observer unsubscribed to ReplaySubject
```

On the [http://reactivex.io/documentation/subject.html](http://reactivex.io/documentation/subject.html) website, there are diagrams to help you understand graphically the interactions between Subjects, Subscribers, and source Observables.

As expected, ReplaySubject will receive all the events submitted to the Subject, even the ones delivered before the subscription was made are received by the observer. After the observer unsubscribes, it stops receiving the events from the Subject.

As an exercise, you can try to create the same for the AsyncSubject and BehaviorSubject.