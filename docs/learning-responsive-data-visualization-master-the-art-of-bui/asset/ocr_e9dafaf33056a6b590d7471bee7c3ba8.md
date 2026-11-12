Note that the + and - operators must be surrounded by whitespace. Otherwise, they will be interpreted as a sign of the second number rather than the operator. Both the other operators * and / don't require a whitespace, but I encourage you to add them for consistency.

We can use these expression in the following snippets:

```
.col-4 {
  width: calc(100% / 3);
}

.col-sp-2 {
  width: calc(50% - 2em);
}
```

The preceding examples look great; however, as we can see in the following figure, we need to be aware of the limitations of browser compatibility:

![](bbox=[0.06, 0.39, 0.9, 0.6])

The cross-browser compatibility of the calc() expression (source: http://caniuse.com/#feat=calc)

# # Responsive charts

Now that we know some basics about absolute and relative units, we can start to define, design, and implement responsive charts. A responsive chart is a chart that automatically adapts its look and feel to the resolution of the user's device; thus, responsive charts need to adapt the following properties:

+   - The dimension (width and height)
+   - The resolution of data points
+   - Interactions and interaction areas