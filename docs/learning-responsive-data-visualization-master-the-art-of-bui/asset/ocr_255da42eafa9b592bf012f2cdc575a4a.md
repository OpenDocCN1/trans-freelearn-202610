There are many reasons why we would like to intercept the chain of fired events and default actions by the browser — to increase performance (as we just said), to disable scrolling when using the same gesture in a visualization, and to fully control the event chain.

JavaScript provides a function to disable the default behavior of the browser while maintaining the execution chain and propagating the event; this function is called `event.preventDefault()`. The default behavior of clicking on a checkbox toggling it, and the default behavior of using the mouse wheel scrolls the page. Whenever we want to prevent these default actions, we use the preceding function.

There is another function that does not affect the default actions of an event but stops the execution chain of the event; this function is called `event.stopPropagation()`. Whenever we want to stop event propagation, we use this function.

If you want to stop all event listeners for a specific event, you can also use `event.stopImmediatePropagation()`. This will stop all custom event listeners for a specific event.

> More information about preventDefault, stopPropagation, and stopImmediatePropagation can be found on MDN at https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault, https://developer.mozilla.org/en-US/docs/Web/API/Event/stopPropagation, and https://developer.mozilla.org/en-US/docs/Web/API/Event/stopImmediatePropagation.

# Touch events in D3

We previously saw that touchmove events don't propagate mousemove events. While both events seem to be identical (one used for mouse and one used for touch), there is a small difference. This difference will be very important to understanding why we should not simply use identical callback handlers that listen for both the events and execute the same function.

To understand the difference between these events, we simply think about their purpose. On Desktop computers, users often move the mouse cursor to reach a certain element on the screen. Therefore, the `mousemove` event is targeted to the closest element underneath the cursor, for example, the mouse is moving over an element.