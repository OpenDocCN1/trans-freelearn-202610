# # Drawing basic shapes

Raphaël provides the `rect`, `circle`, and `ellipse` methods for drawing basic shapes. These are methods of the `Paper` object that is returned when we create our canvas. The `rect` method for example has the following syntax (taken from the Raphaël documentation at http://raphaeljs.com/reference.html#Paper.rect):

```
Paper.rect(x, y, width, height, [r]);
```

The `x` and `y` coordinates describe the top-left point of the rectangle and the fifth parameter, `r`, is the radius of rounded corners where the square brackets indicate that it is optional (by default the corners have a radius of 0). A rectangle of width 50px and height 100px at x = 200px, y = 150px is created as follows:

```
var paper = Raphael('my-canvas', 500, 300);
var rectangle = paper.rect(200, 150, 50, 100);
```

Similarly, we create a square of width 75px with a corner radius of 15px by:

```
var square = paper.rect(300, 150, 75, 75, 15);
```

![](bbox=[0.1777, 0.207, 0.8223, 0.3305])

Note that graphics drawn at negative `x` or `y` points will be drawn starting from outside of the visible region of the canvas meaning that they will be partially or fully hidden from the viewer as illustrated in the following diagram: