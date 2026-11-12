If we execute the preceding function multiple times with different data arguments, we can properly define what happens to the following elements:

+   - Only old (previously existing) elements
+   - Only new elements
+   - All elements
+   - Only removed elements

> There are three great examples about the update pattern on Mike Bostock's Bl.ocks page. I recommend taking a look at all three and trying to understand what exactly is going on in the examples:
> 
> - http://bl.ocks.org/mbostock/3808218
> - http://bl.ocks.org/mbostock/3808221
> - http://bl.ocks.org/mbostock/3808234

# Defining an Identifier for the elements

If we look at the API documentation of the selection.data(data, keyFn) method, we can see that this function takes a second argument called keyFn. With this keyFn argument, one can define an accessor function that identifies the element of the array in a data join.

Let's look at an example where we use the draw(data) function that we implemented in the previous section:

```
// Create 4 elements
draw([0, 1, 2, 3]);
// Update 4 elements
draw([1, 2, 3, 4]);
```

If we don't define a keyFn argument, D3 uses the index of the data array as an identifier for the element. In the preceding example, the function first creates four circles; in the second step, it updates these four circles. However, if we look closely, we see that a part of the data is the same but only shifted to the left. Thus, we would rather want a behavior in which 0 gets removed from the Selection, [1, 2, 3] stays the same, and 4 gets added to the Selection. We can achieve this using the array value as an identifier rather than the array index. Let's define a custom keyFn that returns the array element as identifier.

```
var $$circles = $svg.selectAll('circle')
.data(data, function(d, i){ return d; });
```

In the preceding example, we define an identifier function in the same way as the callback functions that we used in dynamic properties.