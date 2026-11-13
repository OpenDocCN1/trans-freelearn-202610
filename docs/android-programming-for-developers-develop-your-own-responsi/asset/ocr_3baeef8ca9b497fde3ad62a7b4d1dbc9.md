# Summary

We have learned how to draw at low level using a standard view and also a SurfaceView. We have created a sprite class to reuse the code of the items that are displayed on the screen.

The DrawThread has been updated to a more efficient one and we also added a frames-per-second counter to check the efficiency of each configuration.

Along the way, we learned about game controllers and the different ways to create levels. We also decided to spawn asteroids YASS using procedural generation, and put this into effect.

We also added support for parallax backgrounds and layers to the engine.

All in all, YASS is starting to look good, but we are clearly missing something: the bullets do nothing when they hit an asteroid and neither does our spaceship.

It is time to implement collision detection.