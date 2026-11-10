# How to do it...

To import a whole cutscene from a 3D package, follow these steps:

+   1. Create the cutscene in a 3D package. Remember that all the objects' animations should have the same length. They should have keys on the first and last frame.
+   2. If you plan to use **Humanoid** rigs, save every character as a separate file before importing it to Unity.
+   3. Import the cutscene to Unity (and all the characters, if needed). All objects that use **Generic** rigs can be exported together as one file. In such cases, a **Generic** rig will be created for the whole cutscene.
+   4. If you are using Blender and want to export the cutscene to FBX format, use the 6.1 version and make sure to check the **Default Take** option.
+   5. Go to the **Animation** tab in **Import Settings**.
+   6. The **Default Take** holds the whole cutscene animation. You may see additional clips in the Animation tab (especially if you are using skeletal animation). Don't use them. You can remove them from the Import Settings and change the **Default Take** clip name to something more descriptive.
+   7. If you are using **Humanoid** rigs for the characters, import them one by one from the separate files.
+   8. Create an **Animator Controller** for the cutscene and one for every character (if you are using the **Humanoid** rig).
+   9. Place the cutscene animation in the controllers.
+   10. Place all the objects in the same place on the scene.
+   11. If you want to animate cameras, create one camera for every shot of your cutscene.
+   12. Parent the cameras to the animated camera holder objects from the cutscene. You may need to adjust camera rotations.
+   13. Our cameras are children of the camera holder objects (and the cutscene object). But the animation is imported and read-only. So, we cannot change the cameras the same way we did in the *Changing cameras with animation* recipe. We need a script on the root object of the cutscene (the one with the Animator component). Create a new script and call it `CutsceneCameraChanger.cs`. In this script, we have a list of all the cutscene cameras and a `public void ChangeCamera()` animation event that changes the camera based on the given `int newCameraIndex` parameter. It disables all other cameras from the list:

```
public void ChangeCamera (int newCameraIndex)
{
```