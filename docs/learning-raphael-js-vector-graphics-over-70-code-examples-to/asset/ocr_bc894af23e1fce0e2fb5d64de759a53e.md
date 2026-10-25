# # Vector Animation

The star is a polygon whose lines overlap. This makes for an interesting effect when animating the star to an arbelos shape. To demonstrate this, we define a square that when clicked initializes a path animation as follows:

```
```javascript
var square = paper.rect(500, 100, 50, 50).attr({
    'fill': '#09c', cursor: 'pointer'
});
square.click(function() {
    star.animate({
        path: [
            'M', 100, 180,
            'a', 140, 140, 0, 0, 1, 280, 0,
            90, 90, 0, 0, 0, -180, 0,
            50, 50, 0, 0, 0, -100, 0
        ]
    }, 1000);
});
```
```

When running this code, you will see that the triangle is smoothly translated into an arbelos as shown:

![](bbox=[0.2461, 0.459, 0.7539, 0.6562])

There is a particularly good example of path animation on the Raphaël website at http://raphaeljs.com/chart.html. The example demonstrates the cubic Bézier curve animation when toggling through months. We can achieve something similar using the Catmull-Rom curves we encountered in Chapter 3, Drawing Paths. We begin by defining initial and end Catmull-Rom paths and drawing the first one on to our canvas:

```
```javascript
var pathStart = [
    'M', 50, 250,
    'R', 100, 100, 150, 150, 200, 140, 250, 200, 300, 80,
```
```