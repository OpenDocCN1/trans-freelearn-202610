

## Element.getPointAtLength(length)

We can get the _x_ and _y_ coordinates of any point along the length of a path using the getPointAtLength method. This method returns an object with _x_ and _y_ attributes. Using the example of a bowling pin from the previous section, we plot two circular points at the one-eighth and one-half of the total length of the path as follows:

```
var totalLength = pin.getTotalLength();
p1 = pin.getPointAtLength(totalLength / 8);
p2 = pin.getPointAtLength(totalLength / 2);
paper.circle(p1.x, p1.y, 10);
paper.circle(p2.x, p2.y, 10);
```

Another attribute of the object returned by getPointAtLength is the alpha attribute. This returns the positive angle relative to the x-axis subtended by the gradient or derivative line at the point specified as illustrated:

![](img/d8ec653db4cab13fec8b3f5718174af6_57_0.png)

Knowing the derivative at a point is useful as it informs us of the extent to which the arc is changing in _x_ and _y_ at that particular point.

## Element.getSubpath(from, to)

Given a path element, the `getSubpath` method calculates the path string for a subsection of its path. Given two points on a path, it returns the path string that describes the original path between just those two points. Consider the cubic Bézier curve between the points (100, 100) and (300, 100) with anchor points at (100, 200) and (300, 0):

```
var path = paper.path([
	'M', 100, 100, 'c', 0, 100, 200, -100, 200, 0
]);
path.attr('stroke-width', 5);
```

We retrieve the subpath between the point one-quarter along its length to the point at one half of its total length as follows:

```
var totalLength = path.getTotalLength();
var subPath = path.getSubpath(
	totalLength / 4, totalLength / 2
);
```

The `getSubpath` method returns a path string that can be re-used. For example:
`paper.path(subPath).attr('stroke-width', 15);`

This code draws a shorter, thicker section of our original path as shown here:

![](img/d8ec653db4cab13fec8b3f5718174af6_58_0.png)

> There are a number of other utility methods on the `Raphael` object that are particularly useful when working with Bézier curves in the context of transformation and animation. We will look in more detail at some of these methods in the remainder of this book.

## Catmull-Rom curves

In those cases where we need to draw a curve that passes through a prescribed set of points, a Catmull-Rom curve is often appropriate. Catmull-Rom curves are commonly used in charting (that is, in data plotting) and gaming (for example, to move a character along a predefined trajectory).

Raphaël provides a path command to facilitate the drawing of Catmull-Rom curves. The syntax for drawing Catmull-Rom curves is given:

| Command | Parameters | Example |
|---------|------------|---------|
| R or r | x1,y1 (xi, yi)+ | a 25 50 0 1 0 100 200 |

The point (x1, y1) is the second point the curve should pass through while the (xi, yi) points are every subsequent point. The first point should always be our current point, from which the curve starts.

To illustrate Catmull-Rom curves, consider the following data points stored in an array:

```
var data = [
	{x: 50, y: 250},		{x: 100, y: 100},		{x: 150, y: 150},
	{x: 200, y: 140},		{x: 250, y: 250},		{x: 300, y: 200},
	{x: 350, y: 180},		{x: 400, y: 230}
];
```

To plot these data points, we iterate over the array and draw a circle at each x and y position (note that the points are grouped in to a set which makes applying attributes to them easier):

```
var plottedPoints = paper.set();
for(var i = 0, num = data.length; i < num; i+=1) {
	var point = data[i];
	plottedPoints.push(paper.circle(point.x, point.y, 5));
};
plottedPoints.attr({fill: '#09c', 'stroke-width':0});
```

When constructing our path string or path array, we make the current point our first data point using the M command and append the R command to begin drawing our Catmull-Rom curve:

```
var path = ['M', data[0].x, data[0].y];
path.push('R');
```

We then iterate over all remaining points (all points except the first) and append their values to our path array:

```javascript
for(var i = 1, num = data.length; i < num; i+=1) {
  path.push(data[i].x);
  path.push(data[i].y);
}
var curve = paper.path( path );
```

The resultant curve drawn using the `path` command is shown here:

![](img/d8ec653db4cab13fec8b3f5718174af6_60_0.png)

## Summary

We have now covered all the main drawing methods in Raphaël and you should be familiar with the syntax used to draw paths. At this point, it is definitely worth experimenting with the path commands and utility methods we have covered in order to create your own unique graphics.

We are nicely poised to begin looking at interacting with and manipulating our drawings. Up to now, the graphics we have created are drawn once and do not change. In the next chapter we will look at responding to events triggered by a user and transforming the elements that we create.

# 4 Transformations and Event Handling

Having spent the past two chapters drawing static graphics, we will now start to look at how we can transform them and how we can interact with them in the browser.

When you create an element in Raphaël, you are effectively creating a **Document Object Model (DOM)** object. Like the DOM objects you are already familiar with elements drawn on to our canvas will trigger events when interacted with by a user. Raphaël provides a number of convenient, cross-browser methods for interacting with elements and also provides methods that facilitate the drag-and-drop functionality.

Transformations are equally versatile: Raphaël gives us the ability to transform (translate, scale, and rotate) elements with ease using the concept of a **transformation string**.

Over the course of this chapter, we will look at event handling and transformations alongside each other, combining both facets of the library to yield interactive graphics. Overall, we will cover:

- The transform method and basic transformations
- Matrix transformations
- Registering and unregistering basic event handlers
- Achieving the drag-and-drop functionality

## Basic transformations and event handling

We perform transformations in Raphaël using the `transform` method on elements, which accepts as an argument a transformation string. Transformation strings define a sequence of transformations to take place on a particular element.

You can also transform elements and sets using the `transform` attribute. This is often convenient when we are changing multiple attributes at the same time. You will see examples of this in Chapter 5, Vector Animation.

### Basic transformations

Like a path string, a transformation string is composed of a number of commands defined in the order in which they are to be interpreted. The syntax for the basic commands is as follows, where brackets indicate optional parameters:

| Command | Parameters | Example |
|---------|------------|---------|
| T or t | x, y | t 50,100 |
| R or r | angle of clockwise rotation, (rotation_point_x, rotation_point_y) | R 45,0,0 |
| S or s | scale_x, scale_y, (scale_point_x, scale_point_y) or scale_factor, (scale_point_x, scale_point_y) | S 2,4.5,75,125 |

As with a path string, transformation strings have uppercase and lowercase variants. The uppercase variant means that we transform, irrespective of the previous transformation, while the lowercase variant takes previous transformations into account. The following examples demonstrate the basic transformations.

## Translation

First off, consider drawing a triangle and creating a copy of it using the `clone` method:

```
var triangle = paper.path("M 150 20 1 100 160 1 -200 0 z").attr({
 'stroke-dasharray': '- ',
 'stroke-width': 5
});
```

```
var triangleCopy = triangle.clone(); // Clones our object
triangleCopy.attr({
 'stroke-dasharray': ''
});
```

When cloned, the copied triangle has all the same attributes as the original triangle so we unset `stroke-dasharray` to give it a solid outline. We then perform a translation of 300px on the x-axis and 0px on the y-axis as follows:

```
triangleCopy.transform("T 300 0");
```

This has the effect of moving our cloned triangle in `x` as shown in the following diagram:

![](img/d8ec653db4cab13fec8b3f5718174af6_63_0.png)

Note that while the `clone` method is convenient, it is known to affect performance adversely when cloning multiple elements. When cloning multiple elements, creating a new element using the original path is advised.

## Rotation

Creating another cloned triangle now and transforming it with the transformation string below gives us a triangle rotated 45 degrees clockwise about its own center point (the default when we do not specify a rotation point) after being translated 350px in x and -35px in y.

```
triangleCopy.transform("t 350 -35 r 45");
```

