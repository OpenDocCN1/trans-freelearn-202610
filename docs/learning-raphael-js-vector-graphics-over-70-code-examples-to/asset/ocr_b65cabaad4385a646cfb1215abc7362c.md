# # Catmull-Rom curves

In those cases where we need to draw a curve that passes through a prescribed
set of points, a Catmull-Rom curve is often appropriate. Catmull-Rom curves are
commonly used in charting (that is, in data plotting) and gaming (for example, to
move a character along a predefined trajectory).

Raphaël provides a path command to facilitate the drawing of Catmull-Rom curves.
The syntax for drawing Catmull-Rom curves is given:

| Command | Parameters | Example |
|---------|------------|---------|
| R or r | x1,y1 (xi, yi)+ | a 25 50 0 1 0 100 200 |

The point (x1, y1) is the second point the curve should pass through while the
(xi, yi) points are every subsequent point. The first point should always be our
current point, from which the curve starts.

To illustrate Catmull-Rom curves, consider the following data points stored in
an array:

```
```
var data = [
	{x: 50, y: 250},		{x: 100, y: 100},		{x: 150, y: 150},
	{x: 200, y: 140},		{x: 250, y: 250},		{x: 300, y: 200},
	{x: 350, y: 180},		{x: 400, y: 230}
];
```
```

To plot these data points, we iterate over the array and draw a circle at each x and y
position (note that the points are grouped in to a set which makes applying attributes
to them easier):

```
```
var plottedPoints = paper.set();
for(var i = 0, num = data.length; i < num; i+=1) {
	var point = data[i];
	plottedPoints.push(paper.circle(point.x, point.y, 5));
};
plottedPoints.attr({fill: '#09c', 'stroke-width':0});
```
```

When constructing our path string or path array, we make the current point our first
data point using the M command and append the R command to begin drawing our
Catmull-Rom curve:

```
```
var path = ['M', data[0].x, data[0].y];
path.push('R');
```
```