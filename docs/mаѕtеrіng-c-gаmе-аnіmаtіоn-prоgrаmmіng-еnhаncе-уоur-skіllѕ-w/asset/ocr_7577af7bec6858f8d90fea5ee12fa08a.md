# Extending the node tree to support level geometry collisions

Thanks to the good foundation built in the last chapters, adding a new collision type takes a matter of minutes by following these steps:

+   1. First, we add the new event type to the nodeEvent enum in the Enums.h file:
+   ```
+   enum class nodeEvent : uint8_t {
+   ...
+   instanceToLevelCollision,
+   NUM
+   };
+   ```
+   2. We also add some text to the micNodeUpdateMap during the init() method of the renderer to add a name in the node tree:
+   ```
+   mModelInstCamData.micNodeUpdateMap[
+   nodeEvent::instanceToLevelCollision] =
+   "Inst to Level collision";
+   ```
+   3. Then, in checkForLevelCollisions(), trigger the new event if we have at least one colliding triangle:
+   ```
+   if (collidingTriangles.size() > 0) {
+    mModelInstCamData.micNodeEventCallbackFunction(
+    instSettings.isInstanceIndexPosition,
+    nodeEvent::instanceToLevelCollision);
+   }
+   ```
+   4. The event informs the node tree of the instance that a collision with level geometry occurred. As an example, we could just let the instance turn around by 180 degrees, as shown in Figure 12.2: