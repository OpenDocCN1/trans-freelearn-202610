
## Bézier curves

The goal is to create and handle polynomial functions with ease that correspond to a certain progress over time. Creating linear functions such as the linear easing function that we used in the preceding example is straightforward; you learned this in high school mathematics. However, when we think about quadratic or cubic functions (degree 2 or 3), this gets much more complicated. It's not straightforward anymore to create a function to describe a desired shape.

Let's look at the equation of a cubic polynomial function:

```
f(x) = a \cdot x^3 + b \cdot x^2 + c \cdot x + d
```

Now, I would ask you to find the a, b, c, and d parameters such that the polynomial function goes through the (0,0) point and accelerates towards the (1,1) point. This is similar to the progress of the function in the following figure:

![](img/72559bd4713b2b89631dcca187d7e2db_154_0.png)

The cubic easing function (source: http://cubic-bezier.com/#.5,0,1,.5)

You will soon find out that, although it is not impossible to create a function for the preceding figure, it is very nasty and not intuitive using a simple polynomial function. The reason for this is that changes of the coefficients a, b, c, or d lead to unpredictable changes of the described function.

Luckily, the clever guys called de Casteljau and Bézier developed the concept of Bézier curves — a mathematical model for describing polynomial functions by the so-called control points. Bézier curves are parametric polynomial functions that are described by a parameter t in the range of [0, 1] and a certain number of control points (four control points for a polynomial of degree 3). The resulting function is the interpolation between the control points at a position where, at t=0, the function matches the first control point and, at t=1, the function matches the last control point.

The general equation of a Bézier curve is denoted as follows:

```
f (t) = \sum_{i=0}^{n} B_{i,n} (t) \cdot P_i
```

Here, B is the Bernstein polynomial:

```
B_{i,n} (t) = \binom{n}{i} t^i (1-t)^{n-i}
```

The cubic polynomial function from the preceding can now be written as a parametric Bézier curve:

```
f (t) = (1-t)^3 \cdot P_0 + 3t(1-t)^2 \cdot P_1 + 3t^2 (1-t) \cdot P_2 + t^3 \cdot P_3
```

Here, P0, P1, P2, and P3 are the control points of the curve. Now, we can create the easing function that we want by simply moving the control points at the desired position and defining the curvature of the polynomial. In the preceding figure of the cubic polynomial, we can see the starting and end points fixed to (0,0) and (1,1) and the 2 remaining control points in turquoise and magenta that define the progress of the easing function.

In the following figure, we can see how a cubic polynomial function can easily be adjusted by simply defining the control points. We also observe that the curve goes through the start and ending points and is smoothly interpolated between the control points. We can also see that the resulting function (in blue) is simply an interpolation between the control points:

![](img/72559bd4713b2b89631dcca187d7e2db_156_0.png)

Bézier curve interpolated between the control points

Now, we have the knowledge and tools to write all the possible easing functions that we can think of by simply defining control points and using Bézier curves. The interactive tool from http://cubic-bezier.com/ allows you to easily create custom Bézier curves and easing effects.

> If you look for the implementations of easing functions rather than Bézier curves, take a look at the jQuery easing plugin by Dan Rogers on Github at https://github.com/danro/jquery-easing/blob/master/jquery.easing.js.

## Easing functions in D3

D3 supports multiple out-of-the-box easing functions that help to make your animations and transitions more plausible. We can create an easing function using the `d3.ease(type)` function. Here, `type` is one of the following easing types:

- `linear`: This is the linear easing function
- `quad`: This is the quadratic easing function
- `cubic`: This is the cubic easing function
- `sin`: This is the sine easing function
- `exp`: This is the exponential easing function
- `circle`: This is the quarter circle easing function
- `bounce`: This is the easing function with the bouncing effect

By default, the easing functions use the easing mode called `in`; this means that the easing motion refers to the element sliding `in` to the target. The easing mode can also be modified by simply appending on the following modes the name of the easing function:

- in: element sliding into the target
- out: element sliding out of the source
- in-out: element sliding out of the source and into the target
- out-in: inverse easing of in-out

The default easing function is `cubic-in-out`, which corresponds to a slow sliding out and sliding in animation. Let's look at a simple example:

```
<svg width="800" height="600">
<rect x="50" y="60" width="100" height="100"></rect>
</svg>
<script>
var rect = d3.select('rect:nth-of-type(1)');

animate(rect, 'x', 50, 650);

function animate(elem, attributeName, startValue, endValue) {
var duration = 1000;
var interpolate = d3.interpolateNumber(startValue, endValue);
var ease = d3.ease('cubic-in-out');

d3.timer(function(time) {
var t = ease(time/duration);
elem.attr(attributeName, interpolate(t));

if (t >= 1) {
elem.attr(attributeName, startValue);
return true;
}
});
}
</script>
```

In the preceding code, we create an easing function by calling the `d3.ease('cubic-in-out')` method. Now, we can use the function to create and modify the `t` parameter for the interpolation function according to the easing type.

The following figure shows an overview and comparison of multiple easing types; you can find an animated example in the code that comes with this book:

![](img/72559bd4713b2b89631dcca187d7e2db_158_0.png)

> You can find more information about D3 easing functions on the Github page of the project at https://github.com/mbostock/d3/wiki/Transitions#d3_ease. A great showcase of D3 easing function can be found on Katsumi Takano's Blocks at http://bl.ocks.org/hunzy/9929724.

## Transitions

Until now, you learned a lot about animation, timers, interpolation, and easing, but does it really need all of this to do simple animations in D3. No; luckily, not as long as we use simple transitions. A Transition is an animation from a start state to one end state. The term is often used for animations in general and vice versa, but I want to emphasize that a transition is a simple animation that is defined by two states only; here, the interpolation between these states is automatically created from these states.

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
