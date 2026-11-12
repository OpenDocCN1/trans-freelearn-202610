D3 has a very powerful support to create transitions. The only thing we have to do is to provide the starting state, call the .transition() method on a selection, and finally provide the ending state and D3 will take care of the rest. The .transition() method will return an object that is very similar to a selection and has most of its methods. In addition, it provides the .duration(duration), .delay(delay), and .easing(easingType) functions to define the duration time, delay time, and the easing type of the transition.

Great, let's see an example where we use transitions instead of custom animations:

```
<svg width="800" height="600">
<rect x="50" y="60" width="100" height="100"></rect>
</svg>
<script>
var rect = d3.select('rect:nth-of-type(1)');

animate(rect, 'x', 50, 650);

function animate(elem, attributeName, startValue, endValue) {
  elem
  .attr(attributeName, startValue)
  .transition()
  .duration(1000)
  .attr(attributeName, endValue);
}
</script>
```

In the preceding code, we can see how easy it is to implement animations as simple transitions in D3. Depending on the startValue and endValue attributes, D3 tries to figure out which interpolation method it needs to use to create the animation.

Let's look at the code from our bar chart component; here is the snippet to remember the most important part where we update the dimensions of the bars:

```
$$bars
.attr('x', function(d) { return xScale(d.x) + i*barWidth; })
.attr('y', function(d) { return yScale(d.y); })
.attr('height', function(d) { return (yScale(0) - yScale(d.y)); })
.attr('width', barWidth)
.attr('fill', colors(data.key));
```

Let's add a little tweak, so the bars automatically update with a nice animation. Using transitions, we only have to define a starting and ending state:

```
$$bars
.attr('x', function(d) { return xScale(d.x) + i*barWidth; })
.attr('y', function(d) { return yScale(0); })
```