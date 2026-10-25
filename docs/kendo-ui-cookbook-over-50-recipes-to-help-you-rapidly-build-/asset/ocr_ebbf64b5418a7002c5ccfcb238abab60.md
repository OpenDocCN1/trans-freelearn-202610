# # Creating flow diagrams using Kendo Diagram

Adding workflow diagrams or hierarchical structure to a web page is now made easy with
the introduction of diagrams in the Kendo UI library. In this recipe, we will build a flowchart
diagram by creating shapes and then we will connect those with connectors. The library also
provides a set of layouts that can be used to represent a hierarchical or tree-like structure.

# ## How to do it...

A flowchart is used to represent steps in an algorithm; these steps are shown as shapes
(rectangles and circles). The connectors are then used to connect the shapes and describe
the flow. A diagram in Kendo UI is created by invoking the kendoDiagram function on the
selected DOM node:

```
```
var diagram = $("#chart").kendoDiagram({
 connectionDefaults: {
  stroke: {
   color: '#F00'
  },
  startCap: 'FilledCircle',
  endCap: 'ArrowEnd',
 }
}).getKendoDiagram();
```
```

In the preceding code snippet, an empty diagram is created, with some default attributes for
connections specified. A connection is used to connect two or more shapes in the diagram.
The connectionDefaults option specifies the color of the stroke to be applied; the
startCap and endCap options are used to specify the decoration to be applied. In this case,
the startCap option has the value FilledCircle, which will show you a circle at the start
of the connection. The other value that can be assigned to startCap is ArrowStart. This will
add an arrow directed towards the shape. The endCap option has its value set to ArrowEnd.
In this example, the shape from which the connection is initiated will have a filled circle attached
to it, and an arrow is shown, which is directed towards the second shape.

The getKendoDiagram function will return an instance of the diagram once it's initialized.
The diagram is like a canvas on which you can add several shapes and also connect them.