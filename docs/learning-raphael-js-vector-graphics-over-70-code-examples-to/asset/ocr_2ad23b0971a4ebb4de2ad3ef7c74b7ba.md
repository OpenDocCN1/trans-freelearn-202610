We print text using our custom font onto our canvas as follows:
```javascript
var text = paper.print(
    50,
    50,
    'Vi veri veniversum vivus vici',
    paper.getFont('Pacifico'),
    38
);
```

This draws custom text with a font size of 38px onto our canvas at the point (50, 50).
The `getFont` method simply references the name of the family of the font that you
have embedded, whereas Cufón should have taken care of registering the font
with Raphaël.

Attributes can be applied to custom font text in the same way that we have already
applied them to shapes and text. For example:
```javascript
text.attr({
    fill: '315-#000-#666',
    'stroke-width': 0
});
```

This produces the text shown as follows:

![](bbox=[0.2285, 0.5039, 0.7705, 0.6016])

# # Summary

In this chapter we have covered many examples of basic drawing and painting
in Raphaël in the context of a particular drawing area or canvas. You should now
be comfortable drawing basic vector graphics using the library and exploring the
documentation at http://raphaeljs.com.

In the next chapter, we will look at drawing more complex elements using paths.

[ 30 ]