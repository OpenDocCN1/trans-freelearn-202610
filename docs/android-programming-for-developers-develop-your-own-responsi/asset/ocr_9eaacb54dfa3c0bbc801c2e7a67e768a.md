# # Transforming Observables

Apart from the ability to widely implement the Observable-Subscribe software pattern, the RxJava framework allows us to transform, filter, convert, aggregate, manipulate, and work with the stream of items emitted by observable by using observable operators. These entities are able to completely transform the event stream before the events are delivered to the final Subscriber.

RxJava comes with a handy collection of operators that are able to transform the event's content and control the time that the event is delivered.

Let's describe the most common operators available on RxJava:

+   - map: Applies a function to each item emitted and emits the result of the function as a new item.
+   - flatMap: Applies a function to each item emitted by the source Observable where the function returns an Observable that could emit a different number of items or a different type of event.
+   - filter: A transformation operator that uses a function that verifies if each item emitted by the source Observable satisfies a condition. If the condition passes the item, it is forwarded to the following Subscriber.
+   - first: Emits only the first item emitted by the source Observable.
+   - count: Emits the number of items received from the original Observable.
+   - zip: Combines the emissions of two Observables using a function that receives the N item of each original Observable as an argument.
+   - contains: Emits a Boolean event that indicates whether the source Observable has a specified Object.
+   - merge: Merges the events of multiple observers into one event stream.
+   - delay: Delays the emission of an item by a specified amount of time.

For a more complete, detailed, and up-to-date list of operators supported by RxJava, check the RxJava Wiki on GitHub (https://github.com/ReactiveX/RxJava/wiki/Alphabetical-List-of-Observable-Operators).

The RxJava operators generally process an Observable and return an Observable. This design feature allows us to chain the operators and create a