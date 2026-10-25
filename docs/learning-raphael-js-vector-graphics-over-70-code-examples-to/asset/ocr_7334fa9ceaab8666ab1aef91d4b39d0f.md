# Drawing Paths

The c command requires that we specify both control points. The following code
has the effect of drawing a curve from the point (200, 20) to (200, 120) via the control
points (400, 140) and (400, 180). Note also that we have closed the path and applied
an attribute fill:

```
paper.path([
	'M', 200, 20,
	'C', 400, 140, 400, 180, 200, 120,
	'z'
]).attr({fill: '#222'});
```

The result is shown as follows:

![](bbox=[0.1777, 0.335, 0.8223, 0.5078])

The s command has a similar effect to the T command in automatically creating a
control point reflected about the current point. What it actually does is to reflect the
second control point about the current point and this becomes the first control point
for our new subpath, for which we now have to specify our second control point.
The code given here extends the previous example with a subpath drawn using
the s command:

```
paper.path([
	'M', 200, 20,
	'C', 400, 140, 400, 180, 200, 120,
	'S', 0, 100, 200, 220,
	'z'
]);
```

The s path starts from the current point (200, 120) and finishes at (200, 220) as shown.
The second control point from our first curve, defined at (400, 180), is reflected about
the current point, which creates a control point at (0, 60). We have then specified a
second control point at (0, 100):