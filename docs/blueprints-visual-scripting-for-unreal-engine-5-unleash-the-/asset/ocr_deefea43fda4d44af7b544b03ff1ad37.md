+   * Impact Normal: The normal vector that is perpendicular to the surface that was hit
+   * Hit Actor: The reference to the actor hit by the trace

In the next sections, we will see the trace functions available in Blueprint.

# Traces for objects

The LineTraceForObjects function tests for collisions along a defined line and returns a Hit Result structure with data for the first actor hit that matches one of the Object Types values specified in the input parameter.

The MultiLineTraceForObjects function has the same input parameters as the LineTraceForObjects function. The difference between the functions is that the MultiLineTraceForObjects function returns an array of Hit Result structures describing all actors hit by the trace, rather than a single result, making it more expensive to perform. The following screenshot shows the two TraceForObjects functions:

![](bbox=[0.126, 0.525, 0.869, 0.825])

Figure 14.26 – TraceForObjects functions