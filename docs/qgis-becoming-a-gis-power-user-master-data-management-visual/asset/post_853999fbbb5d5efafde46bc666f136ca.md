
## Handling transparencies

Transparency is a lack of pigment or color, such that you can see through one feature to the feature beneath. You can think of this as being similar to tinted or stained glass; some light is allowed to pass through and reflect off what's inside. When used right, transparency can help emphasize or de-emphasize features in a map composition. It can also be used to blend two layers to look as if they are one layer.

## Getting ready

This recipe demonstrates transparency for both vectors and rasters, so we'll need an example of each. The lakes.shp and elevlid_D782_6.tif layers will work well for demonstration purposes. Load both of these layers in a fresh project, putting lake.shp on top.

## How to do it...

- 1. With a vector layer loaded, open Properties and the Style tab.
- 2. On the right-hand side of the dialog, you will see a Transparency slider at 0% (this means 100% solid or opaque).
- 3. Adjust the slider to the right and apply the changes to see them in the map:

![](img/2e88bf5938bf48a409c0a534a8775b21_702_0.png)

Using a bold or dark color will make it easier to notice the change. You will also notice that the Simple fill option shows the original color.

- 4. Now to demonstrate this on a raster, first reset the lakes back to 0%.
- 5. Swap the order in the Layers list so that elevlid is on top.
- 6. Now, open Properties of elevlid and the Transparency tab.
- 7. The Global transparency option will change the value evenly for the whole raster. Set it to 50% and apply it. You should now be able to see the lakes layer, which was hidden below:

![](img/2e88bf5938bf48a409c0a534a8775b21_703_0.png)

No data value is always 100% transparent no matter what Global transparency is set to. Use this to easily eliminate values that you don't want to show up at all.

- 8. (Optional) You may have noticed that below the Global transparency slider is Custom transparency options. This will allow you to make particular values more transparent than others. You can either assign specific values to specific transparencies, or you can add a band to the raster (or use a multiband raster), which specifies the amount of transparency to apply to the rest of the raster (some data formats, such as GeoTiff, call this an Alpha Transparency band):
  - Reset the global back to 0% (otherwise, this is applied in too)
  - Use the green + sign to add some values

## How to do it...

Using the two raster layers, we can try the different blending modes. Of course, this works for vector layers, as well:

- 1. Double-click on the effects layer to open Layer Properties.
- 2. You can find the blending settings by going to Layer Properties | Style | Color Rendering together with other helpful controls for Brightness, Contrast, Saturation, and more, as shown in the next screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_706_0.png)

- 3. Change the Blending mode and click on Apply to see the results.
- 4. Similarly, for vector layers, such as our blending layer, we find the blending mode settings by going to Layer Properties | Style | Layer rendering, as shown in the following screenshot:

![](img/2e88bf5938bf48a409c0a534a8775b21_706_1.png)

The main difference is that, for vector layers, we can control how features are blended together, and how the result is then blended to the underlying layers using the Feature blending and Layer blending modes, respectively. The feature blending mode is applied on a per-feature-basis.

## Cartography Tips

- - **Dodge**: The **Dodge** mode will brighten and saturate the lower layer based on the lightness of the upper layer. This means that brighter colors in the upper layer cause the saturation and brightness of the lower layer to increase. This works best if the top pixels aren't too bright; otherwise, the effect is quite extreme.
- - **Addition**: The **Addition** mode adds the pixel values of both layers. If the result exceeds 1 (in the case of RGB), the respective areas are displayed in white.
- - **Darken**: The **Darken** mode creates a result that retains the smallest RGB components of both layers. Therefore, this is the opposite of the Lighten mode and, just as with Lighten, the results tend to be jagged and harsh.
- - **Multiply**: The **Multiply** mode multiplies the values of both layers, thus resulting in a darker picture.
- - **Burn**: The **Burn** mode causes darker colors in the upper layer to darken the lower layer. **Burn** can be used to tweak and colorize underlying layers.
- - **Overlay**: The **Overlay** mode combines the **Multiply** and **Screen blending** modes. As a result, light parts become lighter and dark parts become darker.
- - **Soft light**: The **Soft light** mode is very similar to **Overlay**, but it uses a combination of **Burn** and **Dodge**. This is supposed to emulate shining a soft light on an image.
- - **Hard light**: The **Hard light** mode is also very similar to **Overlay**. It is supposed to emulate projecting a very intense light on an image.
- - **Difference**: The **Difference** mode subtracts the values of the upper layer from the lower layer (or the other way around) to always get a positive value. Blending with black (which has an RGB value of 0,0,0) produces no change.
- - **Subtract**: The **Subtract** mode subtracts the values of one layer from the other. In the case of negative values, black is displayed:

![](img/2e88bf5938bf48a409c0a534a8775b21_709_0.png)

Overview of the 12 blending modes (background maps "Watercolor" and "Toner" by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under CC BY SA): first row: Lighten, Screen, and Dodge; second row: Addition, Darken, and Multiply; third row: Burn, Overlay, and Soft light; fourth row: Hard light, Difference, and Subtract.

## Saving and loading styles

What's better than making an awesome style for your feature layers? Being able to easily share and reuse them. Both vector and raster styles can be saved and reused—however, in slightly different ways.

## Getting ready

For this recipe, you need two similar vector layers and a set of two similar raster layers. In the example data that is provided, use two of the bus route shapefiles and two of the elevation rasters (for example, elevlid_D782_6.tif).

```
elevlid_D782_6.tif
```

## How to do it...

First we'll start by copying and pasting styles for vector layers:

- 1. Load up two bus route shapefiles and two elevlid rasters.
- 2. The simplest method is to copy styles for vectors or rasters. Just right-click on the layer name in the list and select **Copy Style** from the **Style** menu. Then, right-click on the layer that you want to apply this to and select **Paste Style** from the **Style** menu. You can only copy styles between layers of the same type (for example, **Point to Point**):

Try to copy and paste the style of one bus route to the second bus route using the right-click menus:

![](img/2e88bf5938bf48a409c0a534a8775b21_710_0.png)

- 3. Now to export styles for later use, right-click on a layer and navigate to Properties | Symbology.
- 4. In the bottom-right, there is a Save Style button in the Style menu. The output choices are QGIS Layer Style File... (aka .qml) or SLD File.... Both formats are XML text files, QGIS Layer Style... is recommended for maximum compatibility:

![](img/2e88bf5938bf48a409c0a534a8775b21_711_0.png)

- 5. SLD is compatible with some other web map systems, but it will not capture your QGIS style 100% except in the simplest cases (not all the same options exist in SLD). QML is the native QGIS style file. Note the Load style option for later usage.
- 6. Go ahead and apply a new style to one of the bus route layers.
- 7. Then, save the symbology to QGIS Layer Style File (qml).
- 8. In the property dialog of the second bus route file, click Load Style... and pick the file that you just saved.

> You can open this file in a text editor and make customizations or, for example, batch-find and replace values.
