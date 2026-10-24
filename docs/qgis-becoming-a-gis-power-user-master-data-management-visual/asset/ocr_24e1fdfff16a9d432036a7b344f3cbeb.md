# There's more...

Overview maps are a great way to provide context to more detailed main maps. To add an overview map (as shown in the upper-right corner of the composition in the following screenshot), you need to add a second map item to the composition. To turn this map item into an overview map, go to **Item properties** | **Overviews** and click on the button with the green plus sign. This will add an `Overview 1` entry and enable the **Draw "Overview 1" overview** configuration GUI:

+   - **Map frame**: The **Map frame** drop-down list enables us to define the main map that should be referenced by the overview map. By default, the map items are named `Map 0`, `Map 1`, `Map 2`, and so on, depending on the order they were added to the composition. Therefore, we will select the `Map 0` entry if the main map was the first item that was added to the composition.
+   - **Frame style**: The **Change ...** button can be used to choose a style for the overview frame. Usually, this will be a simple fill with transparency.
+   - **Blending mode**: These are supported by overview frames, as explained in detail in the *Understanding the feature and layer blending modes* recipe.
+   - **Invert overview**: Enable the **Invert overview** checkbox if you want to apply the overview frame style to the areas outside the extent of the main map.
+   - **Center on overview**: Enable the **Center on overview** checkbox if you want the overview map to automatically pan to center on the extent of the main map.