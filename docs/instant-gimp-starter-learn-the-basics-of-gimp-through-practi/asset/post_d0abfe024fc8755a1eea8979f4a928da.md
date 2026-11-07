
13. Let's draw the guide for the speedometer needle; we only need to find the center of the circle.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_47_0.png)

14. Create a needle that points to zero, and color them according to the animal colors.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_47_1.png)

15. Now rotate the needle to show the speed of the animal. Add a drop shadow right after that. Click on Filters | Light and Shadow | Drop Shadow to add the drop shadow.

> While we're on the subject:
> How do I calculate the rotation angle of the needle? Consider the following values.
> Speed of elephant = 25
> Full quadrant indicator = 120
> Which makes rotation: 25/120 × 360° = 75°

The result is shown in the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_48_0.png)

The overall picture would look like the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_49_0.png)

16. Now we are going to create an indicator for the height of the animals. For that, we will use the pencil tool to create a straight line.

17. Click at a point, hold down the Shift key, and it will form a straight line.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_50_0.png)

18. Add a text to make clear that each image represent height and speed of the animals.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_51_0.png)

19. Use your creativity to make the title and the legends for the color-coded animals, as shown in the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_52_0.png)

20. Finally, the infographic is done, as shown in the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_53_0.png)

## Combining multiple pictures

In this section, we will learn how to produce a picture composed of multiple pictures.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_54_0.png)

To do so, follow these steps:

- 1. First, we choose a picture and open it as layers, as shown in the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_54_1.png)

Following is the picture I selected:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_55_0.png)

2. Add an alpha channel to the picture so that it exposes transparency, as shown in the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_55_1.png)

- 3. Use a magic wand to select the sky.
- 4. Cut it out and paste it as a new layer.
- 5. Shift the layer backward, so it lies at the back.

Leave the picture for a while; we are looking into drawing a monster on the background. I drew a picture of a monster on my sketch book and scanned it. Therefore, I can open it using GIMP. We can use threshold setting to get the outline clearer.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_56_0.png)

To draw a picture on the background, follow these steps:

- 1. Use Select by color to select the outline of the monster.
- 2. Clean up the picture, and delete the background, so we can only get the monster.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_57_0.png)

3. Copy the monster and paste it over in the picture we were working on earlier.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_57_1.png)

4. Anchor the layer, and adjust the layer so that the monster falls at the back of the scenery.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_58_0.png)

5. Next step is to create the fire. For that, create a new layer, and select a portion of the layer near the mouth of the monster. Fill the selection with a gradient, as shown in the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_58_1.png)

6. Deselect the rectangle, and select a bigger portion of the layer.

7. Go to Filters | Distorts | IWarp to shape the fire to the gradient. Smudge the gradient pointing up. Use the Swirl CCW or Swirl CW option to create a beautiful effect of fierce fire.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_59_0.png)

8. Then click on the Move radio button, as shown in the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_59_1.png)

9. Now that we have a fire-breathing monster, try to do some adjustment to make the fire slanting to the position of the head. Use the Move Tool and the Rotate Tool options to do so.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_60_0.png)

Now let's color the monster. To do so, follow these steps:

- 1. Apply a gradient on the monster.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_61_0.png)

- 2. Finally, give the monster some shades using the Airbrush Tool option.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_62_0.png)

Now, we came up with a fire-breathing monster, terrorizing the neighborhood.
How cool is that!

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_63_0.png)

## Plugins

Plugins are a very interesting part of GIMP. I am not going to explain how to install plugins step by step. But, the plugin being advanced, I will have to explain how to install one.

Plugins can bring new functionalities to GIMP. It might be a basic batch rotation or a fantastic vector drawing plugin that draws graphics based on complex mathematical calculation.

## Instant GIMP Starter

I will be using my own watermarking plugin that puts a watermark on a batch of images. Follow these steps:

- 1. Browse to https://github.com/fazreil/GIMP-Watermarker/blob/master/fazreil.scm.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_64_0.png)

This page shows a plugin I made for GIMP.

- 2. Now, download it from the page.

3. Unzip the downloaded file to the script directory in your GIMP installation folder. My setup follows the path, C:\Program Files\GIMP 2\share\gimp\2.0\scripts, as shown in the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_65_0.png)

4. Restart GIMP 2.0.

5. After you have restarted the server, it will look like the following screenshot, with a new item in the menu:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_65_1.png)

There are many places where people keep their written plugins; you might find them if you search in a search engine or head to http://registry.gimp.org/. Happy hunting and installing!

## Using filters

Filters are a very cool feature in GIMP. We've used some filters in our work before (like the IWarp when creating the fire effect). This time we are going to use more filters to create the following picture:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_66_0.png)

To create the previous picture using filters, follow these steps:

- 1. To create a new image, follow the settings shown in the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_66_1.png)

- 2. Render a blue fog as the background. Click on **Filters | Render | Fog**, and you will be presented with the following menu:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_67_0.png)

You will have a very random pattern of fog, as shown in the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_67_1.png)

- 3. I am going to paste the GIMP boy as a new layer to this drawing.
- 4. Copy and paste a picture of your choice into a new layer.
- 5. Use the Scale Tool option to shrink him to a smaller scale. Hold down Ctrl while dragging, to maintain the aspect ratio. In the following screenshot, notice that the GIMP boy is too big to fit in this picture.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_68_0.png)

6. Add a lighting effect by clicking on Filters | Light and Shadow | Lighting Effects.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_68_1.png)

7. Now, the surrounding looks better with the effect. Next, add in Fin just for fun, as shown in the following screenshot:

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_69_0.png)

8. Use a new layer, and fill the bottom part with a gradient. This is an easy way to render grass on the canvas.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_69_1.png)

9. Use whirl and pinch (Filters | Distorts | Whirl and Pinch) to make the gradient looks wavy.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_70_0.png)

10. Apply a shift effect to put grass on the gradient by clicking on Filters | Distorts | Shifts.

![](img/a7d4ab10f584c5ec31bcd98067c9f4ab_70_1.png)
