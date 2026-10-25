The first (x, y) pair moves the pen to the point (50, 50) on our canvas as we would expect. However, the second (x, y) pair has the effect of drawing a line to the point (100, 150), as well as moving the current point. This behavior means that the moveto command can be used to draw polygons. The following code draws a triangle with vertices at (250, 50), (200, 100), and (300, 100) using only the moveto command:

```
var path = paper.path([
 'M', 250, 50
 200, 100,
 300, 100,
 250, 50
 ]);
```

You should notice that all the (x, y) points mentioned are absolute, that is, defined relative to the origin point of our canvas. Were we to specify the lowercase variant of the command, m, every successive (x, y) pair would be relative to the previous one (but note that the first (x, y) pair is always positioned using absolute coordinates regardless of the M command’s case). The same triangle defined using relative coordinates is described by the path string, m 250,50 -50,50 100,0 -50,-50.

A number of examples of the moveto command are shown here:

![](bbox=[0.1836, 0.4719, 0.8184, 0.6422])

# ## The lineto commands

The **lineto** commands draw straight lines from the current point to a point specified. There are three lineto commands:

+   - `L` for drawing lines to any (x, y) point
+   - `H` for drawing horizontal lines in x
+   - `V` for drawing vertical lines in y