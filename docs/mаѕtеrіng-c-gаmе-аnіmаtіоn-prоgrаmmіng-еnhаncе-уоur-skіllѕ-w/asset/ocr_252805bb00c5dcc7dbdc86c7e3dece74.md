For the Vulkan version of the code, the variable will be created in the VKRenderData struct in the VkRenderData.h file.

In the UserInterface class, we add a checkbox for rdDrawLevelAABB in the Levels section of the createSettingsWindow() method, allowing us to toggle the level’s AABB lines.

That’s all! In Figure 11.5, an example map from Sketchfab has been loaded next to the woman model, and the level’s AABB was activated:

![](bbox=[0.13, 0.26, 0.86, 0.63])

Figure 11.5: Example map (unscaled) and an instance of the woman model (source: Counter Strike 1.6 map “de_dust” by Zarudko at https://skfb.ly/6QYJw)

As a gamer, you may know this map: It’s “de_dust” from Counter Strike 1.6. The map is available at Sketchfab in several versions and formats, along with other popular game maps.

The bounding box around the level data helps us to see the dimensions of the level. Especially for unused areas like in the front right of Figure 11.5, finding the boundaries of the level data would be hard without the AABB lines.

You will also notice the unusual size ratio between the level map and the model instance. The map and model have been loaded with their default scaling values, and the size values have been chosen arbitrarily by the author of the files. Resizing one or both objects is easy thanks to the