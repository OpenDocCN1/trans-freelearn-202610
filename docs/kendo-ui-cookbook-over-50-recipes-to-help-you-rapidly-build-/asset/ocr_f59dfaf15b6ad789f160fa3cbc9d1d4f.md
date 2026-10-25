In the preceding code snippet, six shape objects are created by specifying their content text along with the position data. There are two types of shapes that you can create, rectangles and circles. Once the shapes have been created, they are added to the diagram by calling the addShape function on the diagram object.

Once the shapes have been added, connections between them can be established:

```
diagram.connect(shape1, shape2);
diagram.connect(shape2, shape3);
diagram.connect(shape3.getConnector('Right'),
 shape4.getConnector('Top'), {
 points: [
 new kendo.dataviz.diagram.Point(650, 215)
 ]
 });
diagram.connect(shape3.getConnector('Left'),
 shape5.getConnector('Top'), {
 points: [
 new kendo.dataviz.diagram.Point(250, 215)
 ]
 });
diagram.connect(shape5, shape6);
diagram.connect(shape6.getConnector('Bottom'),
 shape3.getConnector('Bottom'), {
 points: [
 new kendo.dataviz.diagram.Point(250, 470),
 new kendo.dataviz.diagram.Point(450, 470)
 ]
 });
```

The connect function accepts two shape objects and an optional configuration object. A connection between the two shapes is added; there are four connector points available on a shape instance, namely, Top, Bottom, Right, and Left. The framework identifies the position of the shapes on the diagram and chooses the correct connector point from the Shape instance. In the preceding example, shape2 is positioned right below shape1, and hence, the connection between the two will be from the shape1 bottom connector point to the shape2 top connector point.

In the third example, the connector point to be used is explicitly mentioned by calling shape3.getConnector('Right') and shape4.getConnector('Top'). Also, the third parameter is the configuration object that is used to configure the connector. The third parameter in the preceding code snippet specifies the points configuration. This is used to set intermediate points of the connection. This will be applied in addition to connectionDefaults, which is specified when initializing the widget.