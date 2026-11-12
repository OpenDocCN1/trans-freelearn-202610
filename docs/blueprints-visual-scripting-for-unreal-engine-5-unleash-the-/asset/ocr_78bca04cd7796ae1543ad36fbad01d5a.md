These are the input parameters of the two TraceForObjects functions:

+   - **Start**: The location vector that defines the start of the line to be used for the collision test.
+   - **End**: The location vector that defines the end of the collision test line.
+   - **Object Types**: An array that contains object types that will be searched with the trace. The trace will ignore all objects of any other types.
+   - **Trace Complex**: A Boolean value. If it is True, then the trace will test against the actual Mesh. If it is False, then the trace will test against simplified collision shapes.
+   - **Actors to Ignore**: An array with actors that should be ignored in the collision tests.
+   - **Draw Debug Type**: This allows the drawing of a 3D line that represents the trace.
+   - **Ignore Self**: A Boolean value that indicates whether the Blueprint instance that is calling the function should be ignored in the collision test.

There are other line trace functions that use Trace Channel instead of Object Types, as we will see in the next section.

# # Traces by channel

The LineTraceByChannel function tests for collisions along a defined line using Trace Channel, which can be set to Visibility or Camera, and returns a Hit Result structure with data for the first actor hit in the collision test. There is also the MultiLineTraceByChannel function, which returns an array of Hit Result structures describing all actors hit by the trace.