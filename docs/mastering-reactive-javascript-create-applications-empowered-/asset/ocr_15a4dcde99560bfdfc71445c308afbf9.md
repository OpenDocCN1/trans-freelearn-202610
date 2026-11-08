# 4

# Transforming Data - Map, Filter,
and Reduce

In the previous chapter, we started discussing the use of RxJS in our programs. To start using it, we compared Observables in Reactive Extensions with EventStreams and Properties in bacon.js. Then we looked at some of the most common sources of data we can use to create Observables, as follows:

+   - Arrays
+   - Range
+   - Interval
+   - Promises and callbacks
+   - DOM events
+   - Any arbitrary source

After this initial overview of the RxJS API on how to create Observables, we learned how to subscribe to them so we are able to take action whenever new data is made available in the Observable. Subscribing to an Observable means being able to not only react to new incoming data, but also to take some action when an error occurs or an Observable finishes. To do this, we learned how to use the `subscribe()` method, using functions or by creating an Observer.

We also learned how to use Subjects. Subjects allow us to create new Observables and keep pushing data to them.