![](bbox=[0.125, 0.088, 0.865, 0.405])

Figure 14.27 – TraceByChannel functions

The input parameters of these functions are as follows:

+   • **Start**: The location vector that defines the start of the line to be used for the collision test.
+   • **End**: The location vector that defines the end of the collision test line.
+   • **Trace Channel**: A channel used for the collision test. It can be **Visibility** or **Camera**. The trace will search for objects that overlap or block the trace channel selected.
+   • **Trace Complex**: A Boolean value. If it is **True**, then the trace will test against the actual Mesh. If it is **False**, then the trace will test against simplified collision shapes.
+   • **Actors to Ignore**: An array with actors that should be ignored in the collision tests.
+   • **Draw Debug Type**: This allows the drawing of a 3D line that represents the trace.
+   • **Ignore Self**: A Boolean value that indicates whether the Blueprint instance that is calling the function should be ignored in the collision