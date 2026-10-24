![](bbox=[0.17, 0.145, 0.83, 0.35])

3. By specifying data-defined placement, the labeling toolbar's tools are now available (note that the editing mode has to be turned on), and we can use the Move label and Rotate label tools to manipulate the labels on the map. The changes are written back to the attribute table.

4. Try moving some labels, especially where they are placed closely together, and watch how the automatically placed labels adapt to your changes.

# Controlling label rendering

In the Rendering section, we can define Scale-based visibility limits to display labels only at certain scales and Pixel size-based visibility to hide labels for small features. Here, we can also tell the labeling engine to Show all labels for this layer (including colliding labels), which are normally hidden by default.

The following example shows labels with road shields. You can download a blank road shield SVG from http://upload.wikimedia.org/wikipedia/commons/c/c3/Blank_shield.svg. Note how only Interstates are labeled. This can be achieved using the Data defined Show label setting in the Rendering section with the following expression:

```
"level" = 'Interstate'
```