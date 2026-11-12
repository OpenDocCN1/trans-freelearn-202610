In the preceding code, we see that we only need to access the dispatch object to register an event listener. It's very common that the method to listen for an event is called `on` and has the signature called `.on(eventName, eventHandler)`. We listen for the `dataClick` event and register an event handler (also called callback or event listener) that logs the argument to the console. Note that we always need to make sure that the component provides a way for other components to register event handlers and hence to access the dispatch object.

Finally, we can trigger the event by simply calling the event as a function on the dispatcher object:

```
// Dispatch event
dispatch.dataClick({foo: 'bar'});
```

We see in the preceding code that we can also send some data together with the event. After triggering the event, all event listeners of this event will be executed with the event data as argument.

Again, D3's event system is perfect to create an event-based architecture for your visualization, and I really encourage you to do so. This makes it much easier to trace function calls through the application, create reusable, and decoupled components, and standardize the communication between the different components.

> You can find more information about the D3 event system on the Github page of the project at https://github.com/mbostock/d3/wiki/Internals#events.

# # Responsive events

Making your visualization appealing on mobile devices does not only mean to make them look good but also to make them feel good or to make them work at all. And an interactive visualization only feels good when you can actually do all the interactions not just with a mouse but also with your fingertip. You learned in the previous chapter that displaying too much data on a low-resolution device is bad because one cannot really differentiate the data points anymore, which leads to poor performance and usability. Another good reason to reduce the resolution of the data for mobile devices is that it automatically makes the interaction areas on the data points bigger.