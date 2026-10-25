

## Inkscape

Inkscape is an open source SVG editor whose feature set is similar to that of Adobe Illustrator. It is used by many designers given that:

- It is open source and actively maintained / developed
- There are a good number of tutorials available available online
- It has a comprehensive feature set
- It conforms to the W3C standards SVG

Inkscape also has the advantage of being cross-platform with releases available for Windows, Mac, and Linux.

## Downloading Inkscape

Inkscape is available for download at `http://inkscape.org/download/`. It is recommended that you download the latest stable release, which is 0.48.4 at the time of writing.

## Getting started with Inkscape

While an in-depth coverage of Inkscape is beyond the scope of this book, we will look at creating some basic SVGs using Inkscape using the following steps:

1. To begin, open up Inkscape and choose **File | New** from the menu. Inkscape comes with a number of predefined document sizes out of the box and you will want to select the one best suited to the graphic that you're creating. For now, choose **default**.

On the left-hand side, you will see that Inkscape has a number of shape drawing tools for drawing basic shapes, text, freehand, and Bézier paths.

2. Choose the circle.
3. Click-and-drag anywhere on your canvas.
4. When releasing your mouse, your circle will look as follows:

![](img/d8ec653db4cab13fec8b3f5718174af6_93_0.png)

Notice the anchor points on the circle. The leftmost point is used to control the width of the circle, the topmost point controls the height while the rightmost point is used to change the end point. Click-and-drag on the rightmost breakpoint: dragging inside the circle draws a straight line from the end point to the start point while dragging outside creates a segment.

## Working with Existing SVGs

The result of dragging outside of the circle by an angle of a few degrees is shown in the following screenshot:

![](img/d8ec653db4cab13fec8b3f5718174af6_94_0.png)

For our web-based projects, the document size is usually going to be that defined by the constituent objects. Click on **File | Document Properties | Page** and then expand the **Resize** page to a content drop-down menu. Here you can define custom widths and heights for the document. Clicking on **Resize page to drawing or selection** will resize the page and the dimensions of the selected element (or all drawn elements if none is selected).

What this actually does is it applies a translation to all elements on the current layers, that is, the paths are drawn and then a transformation is applied. If we want all paths to be drawn relative to the "new" origin point (which is usually the case) the following steps should be performed:

1. Resize the document to the desired size.
2. Go to **Layer | Layers...**
3. Create a new layer by clicking on the **+** symbol on the layers pane.
4. Select all objects in the previous layer.
5. Choose **Layer | Move Selection to Layer Above**.
6. Delete the previous layer.

The result is as follows:

![](img/d8ec653db4cab13fec8b3f5718174af6_95_0.png)

Arguably the best tool we have at our disposal is the pen tool, which is used to draw Bézier curve paths:

1. Select the pen tool (*Shift + F6*).
2. Single click anywhere in the document.
3. Click-and-drag at another point in the document.

## Working with Existing SVGs

You will notice that each click defines an anchor point that defines the extent to which the curve is curved as shown in the following screenshot:

![](img/d8ec653db4cab13fec8b3f5718174af6_96_0.png)

Each successive click defines a new anchor point and the path is closed by joining it to the start point anchor point:

![](img/d8ec653db4cab13fec8b3f5718174af6_96_1.png)

## Inspecting paths

Since we’re familiar with supplying raw path string data to the `path` method in Raphaël, it goes without saying that we can copy a path string from an SVG and draw paths this way. For complex graphics, the raw XML of an SVG is not always easy to work with and manipulating multiple paths can be cumbersome but this approach of working with existing SVGs does give us complete control over the graphics that we are manipulating.

## Inkscape's XML Editor

One of the great features of Inkscape is the ability to view and edit an SVG’s XML markup directly. Consider the example of the circle in the previous section. If you click on **Edit | XML Editor**, you will be presented with the SVG’s document structure. Clicking on the element you have created on the left—the `svg:path` element—will list all of the object’s attributes including the `d` attribute, which is the element’s path prior to the transformations being applied. Clicking on the `d` attribute reveals the path for the element:

![](img/d8ec653db4cab13fec8b3f5718174af6_97_0.png)

## Working with Existing SVGs

If we take this path and use it in Raphaël as follows:

```javascript
var initialPath = 'M 197,94.46 '
 + 'A 98.5,94.5 0 1 1 167.39,26.92 L 98.5,94.46 z';
var pac = paper.path(initialPath);
pac.attr({fill: '#09c', 'stroke-width': 0});
```

Then it is drawn onto our canvas as shown:

![](img/d8ec653db4cab13fec8b3f5718174af6_98_0.png)

Going back into Inkscape, move the arc anchor point so as to close the "mouth" of the segment that we created:

![](img/d8ec653db4cab13fec8b3f5718174af6_98_1.png)

As before, we get the path for this element using the XML Editor.

We can achieve an interesting effect here with relatively little effort using path animation, animating from the former "open mouth" path to the latter "closed mouth" path. To begin, we define a function named move that determines the x and y coordinates of our pac element based on rotation (which is initialized to zero to begin with):

```javascript
var rotation = 0;
var move = function() {
    var x = Math.cos(Raphael.rad(rotation)) * 800;
    var y = Math.sin(Raphael.rad(rotation)) * 800;
}
```

Next we animate the element to our new path or "closed mouth" path:

```javascript
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

## Taking paths from an existing SVG image

Since SVG images are just XML files, you can open them in your text editor in order to obtain the path information. Often it is just as simple as copying and pasting path information from individual `path` elements in the XML. While this is, as mentioned, cumbersome, there are other problems with this approach. The main issue is whether or not elements, or groups of elements, have had transformations applied to them. In the case where they have transformation applied to them, you will need to apply the same transformation to the `path` elements that you create.

## SVG to Raphaël conversion tools

There are a number of good SVGs to Raphaël JS converters out there that will convert an SVG image into Raphaël automatically and without too much pain.

### Ready Set Raphaël

Ready Set Raphaël (http://www.readysetraphael.com) is a web-based SVG converter that allows you to upload an image and grab the resultant Raphaël code. It supports SVG groups, ellipses, polygons, text, circles, paths, and rectangles and parses all associated styling information into element attributes:

![](img/d8ec653db4cab13fec8b3f5718174af6_100_0.png)

To use it, you simply upload an existing SVG image and click on the **Copy the Code** button.

To test this out, download one of the sample files from http://www.readysetraphael.com at the bottom of the page and copy and paste the resultant code into an HTML document between the `<script></script>` tags. Note that you will need to create an HTML element with an ID `rsr`:

