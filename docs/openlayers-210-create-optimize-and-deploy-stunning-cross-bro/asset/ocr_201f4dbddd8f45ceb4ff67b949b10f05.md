# Strategy.Save

Using this strategy, features can be saved to a server. The server must allow saving, such as a WFS server using transactions. When the `save()` method is called, changes will be attempted to be saved to the server.

There is also an `auto` property which can be set to a `{Boolean}` or `{Float}` (in seconds). If set to `true`, which will save immediately after the features are modified (or deleted). If set to a number, the features will be saved at the passed in interval.

# Summary

This ends the chapter on the Vector Layer class. We talked about what the Vector Layer class is and how it works. We also went over the properties, methods, and events of Vector Layer class and demonstrated them with examples. The Feature and Geometry classes were also covered, along with their properties and methods. Lastly, we learned about the Strategy, Protocol, and Format classes and how to create a vector layer with them.

In the next chapter, we'll work more with the Vector Layer. We'll learn how to style our Vector layers, along with learning how to use the Rule and Filter classes to give us complete control over how our vector layer looks.