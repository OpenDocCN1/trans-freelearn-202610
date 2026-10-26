The process for the breakdown of the stream is as follows:

+   1. **SET_PARAMETER** is sent to the server to set a parameter value for a presentation or stream specified by the URI.
+   2. A second **SET_PARAMETER** request is sent to the server.
+   3. The **TEARDOWN** command is sent to close the connection.