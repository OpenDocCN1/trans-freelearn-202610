to zoom into the graph so that the point becomes visible. These buttons are inside the red rectangle in Figure 6.14.

+   6. We want the zoom animation to be quick, so at the top of the Timeline Editor, find the field next to Length and change to value to 0.3 to limit the range of the animation to 0.3 seconds.

+   7. Now, press Shift and click at the end of the light gray area on the right of the graph. Adjust the fields to 0.3 for Time and 45 for Value:

![](bbox=[0.126, 0.287, 0.87, 0.449])

Figure 6.14 – Time and value of the second point

Notice how the line that represents the value gradually slopes down from 90 degrees to 45 degrees. This means that when this animation is called, the player's FOV will smoothly transition from being zoomed out to being zoomed in, rather than a jarring switch between the two values. This is the advantage of using timelines over changing the values directly with a set value Blueprint.

+   8. Now, return to Event Graph. We want to connect our timeline to our set FOV operation, just like what is shown in this screenshot: