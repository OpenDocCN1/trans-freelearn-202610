We can achieve an interesting effect here with relatively little effort using path animation, animating from the former "open mouth" path to the latter "closed mouth" path. To begin, we define a function named move that determines the x and y coordinates of our pac element based on rotation (which is initialized to zero to begin with):

```
var rotation = 0;
var move = function() {
    var x = Math.cos(Raphael.rad(rotation)) * 800;
    var y = Math.sin(Raphael.rad(rotation)) * 800;
}
```

Next we animate the element to our new path or "closed mouth" path:

```
pac.animate({
    path: 'M 197,94.46 ',
    + 'a 98.5,94.5 0 1 1 -2.1e-4,-0.194727, L 98.5,94.46 z',
    transform: ['...T', x, y]
}, 500, function() {
    // Rotate our element and reset the path
    rotation += 90;
    pac.attr({
        path: initialPath
    }).transform(['...R', 90]);
    setTimeout(move);
});
```

We apply a translation based on the x and y coordinates of our element over half a second. Once the animation has finished, we rotate our element. Note that the transformation is prefixed with an ellipsis that ensures that any translation takes the rotation into account after a 90-degree rotation, translations will be along the y-axis. This is repeated ad infinitum by calling setTimeout on our move function.

When this code is run, the "mouth" goes from being open to closed during the length of one animation and is then repeated with an applied rotation. The rotation has the effect of moving our element along the edge of our container (this demo is available in the source code and website accompanying this book).

# # Taking paths from an existing SVG image

Since SVG images are just XML files, you can open them in your text editor in order to obtain the path information. Often it is just as simple as copying and pasting path information from individual `path` elements in the XML. While this is, as mentioned, cumbersome, there are other problems with this approach. The main issue is whether or not elements, or groups of elements, have had transformations applied to them. In the case where they have transformation applied to them, you will need to apply the same transformation to the `path` elements that you create.