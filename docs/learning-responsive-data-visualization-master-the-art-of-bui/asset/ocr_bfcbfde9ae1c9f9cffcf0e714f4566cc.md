```
var value = parseFloat(elem.attr(attributeName));
elem.attr(attributeName, value + valueStep);

if (value >= endValue) {
    elem.attr(attributeName, startValue);
    return true;
    });
}
</script>
```

In the preceding code, we see only very little changes to the previous example. The most obvious one is that we are now dealing with D3 selections instead of the native HTMLNode objects; this means we can use the `.attr()` setter and getter methods. However, observe that we replaced the `setInterval` function with the `d3.timer` method, which not only removes the need for the `intervalId` variable to keep track of the timer but also runs automatically at the fastest framerate possible (using `requestAnimationFrame()` under the hood). We also obtain access to the `time` variable inside the `callback` function, which stores the elapsed time in milliseconds since the animation started. Take a look at the code that goes along with the book to find executable examples of these animations.

In the preceding code, we incrementally add the `valueStep` value to the current `x` attribute of the `rect` element. This means we are doing some kind of manual (linear) interpolation of this value. Using D3's timers, we can now easily extend the preceding example to also use D3's interpolations instead of the manual one.

> You can find information about D3 timers on the Github page of the project https://github.com/mbostock/d3/wiki/Transitions#timers.

# Interpolating attributes using D3

As we saw in the preceding example, writing the linear interpolations of numeric values is not really difficult even if we want the duration of this interpolation to last for a specific amount of time. It can get tricky if we want to interpolate non-numeric values, such as color values, the coordinates in the `d` attribute of a path element, or if we want to implement non-linear interpolations (we will see this in the following section about easing).