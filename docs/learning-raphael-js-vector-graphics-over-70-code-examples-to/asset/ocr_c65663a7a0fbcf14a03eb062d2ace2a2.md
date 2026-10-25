Likewise, a rectangle at (100, 50) is animated to a point (400, 100) in x and y with increased width, reduced opacity, and a larger stroke-width as follows:

```
var rectangle = paper.rect(100, 50, 20, 50).attr({
    fill: '#345'
});
rectangle.animate({
    opacity: 0.6,
    x: 400,
    y: 100,
    width: 100,
    'stroke-width': 10
}, 1000);
```

The result is as follows:

![](bbox=[0.1777, 0.3812, 0.8223, 0.5531])

# # Animating paths

The `path` attribute of an element allows us to animate paths in the same way that we animated attributes in the previous section. While the basic shapes have intrinsic `x` and `y` or `cx` and `cy` attributes and width and height, paths do not and so we will animate custom paths from one path to another.

Consider drawing a star as follows:

```
var star = paper.path(
    'M 100,40 40,210 190,90 10,90 160,210 z'
);
```