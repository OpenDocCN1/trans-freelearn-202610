# # A hover state for touch

We will quickly realize that, when dealing with touch and mouse support, the application state needs to be adapted. A very common case for these states is the hover state, which can be achieved by hovering over an element with the mouse pointer. The following figure shows a typical application of the hover state in a visualization; it reveals more information about the current position by hovering over a data point:

![](bbox=[0.102, 0.215, 0.94, 0.528])

A Hover interaction

While this is a great use of the current mouse pointer position, this state doesn't (usually) exist in only touch devices. Hence, we need to modify this behavior such that even touch devices can access this information. We previously read that detecting touch devices is impossible, and therefore, we cannot simply add a button for touch devices but should instead rethink the interaction.

In the preceding example, we can implement a cursor that we can drag from an axis into the chart. This might not be a perfect replacement for hover states, but it brings us on-track to the new interaction model. We can now argue that a hover event (or a simple tap) on the chart reveals a cursor at this position (or the closest data point). Practically, it is exactly the same interaction as before; however, we now have to also think about how to remove the cursor from the visualization — maybe by moving it back onto an axis. I hope you get the idea; by rethinking the interaction, I mean exactly this — thinking about where the hover state (or tap) can still work and enhance the new interaction by implementing and optimizing the interaction for touch devices from beginning to end.