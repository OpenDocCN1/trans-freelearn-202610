```
attribution: {
    position: "topRight"
},
.
});
```

Now when you render the page, the controls will be positioned accordingly.

![](bbox=[0.152, 0.232, 0.871, 0.649])

The position attribute can have one of the following values: topLeft, topRight,
bottomLeft, and bottomRight. It is also possible to hide the controls by setting its
value to false:

```
controls: {
    navigator: false
},
```

By setting the specified control to false, the control would not be shown on the page.