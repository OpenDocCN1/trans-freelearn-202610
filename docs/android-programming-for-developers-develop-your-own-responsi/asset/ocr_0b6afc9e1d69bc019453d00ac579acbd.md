# Note

SurfaceView is not hardware-accelerated and may perform worse than normal View.

Anyway, you should know both and be able to swap them easily, even if it is just for testing purposes. We will create an interface named GameView, which will be implemented by the two classes, so they can be changed easily. The classes we will make are:

+   - StandardGameView: This will extend from View
+   - SurfaceGameView: This will extend from SurfaceView