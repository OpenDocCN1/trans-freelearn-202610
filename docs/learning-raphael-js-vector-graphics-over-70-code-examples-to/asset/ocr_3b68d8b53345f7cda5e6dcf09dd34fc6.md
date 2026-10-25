# # Animation easing

**Easing** describes how the value of an attribute varies with regard to time. By default,
the value of an attribute changes consistently — that is, _linearly_ — over the course of
an animation but by specifying a particular easing type, we can change the way in
which the attribute is animated. Consider the following graph that demonstrates
three of the available easing types:

![](bbox=[0.1777, 0.2656, 0.8232, 0.4414])

For each easing type, the rate at which the attribute changes varies along the graph in
the time axis. Each easing type can be described as such:

+   - **Linear**: The value varies consistently from its start value to its end value over
+   the course of the animation
+   - **Ease Out**: The value increases quickly towards its end point before slowing
+   down towards the end of the animation
+   - **Ease In Out**: The value decreases slowly at first and then increases quickly
+   before finally slowing down to its end point towards the end of the animation

# # Built-in easing formulas

Raphaël has a number of built-in easing types including those detailed in the
previous section and others such as 'elastic' or 'bounce'. The effect of each is
well-demonstrated at http://raphaeljs.com/easing.html wherein the cx and
opacity of circles are animated with different easing types. As an example, consider
applying an easing type of 'bounce' to our Catmull-Rom path animation curve:
square.click(function() {
curve.animate({
path: pathEnd
}, 500, 'bounce');
});