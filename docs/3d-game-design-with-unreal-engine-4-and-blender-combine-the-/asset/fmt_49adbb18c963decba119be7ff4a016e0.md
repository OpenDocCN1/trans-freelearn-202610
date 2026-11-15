
## UV Unwrapping

3. In the 3D viewport (right side), you will notice that the `Tools` panel has changed to the `Shading/UV` tools. Under the `UVs` heading there is the `UV Mapping` option drop-down menu. These options are where we begin UV unwrapping an object. With all of the polygons selected, open the menu and select the `Smart UV Project` option:

   ![UV unwrapping our game asset](OEBPS/Images/BO5189_03_20.jpg)

   A basic UV layout using Smart UV Project

4. `Smart UV Project` is Blender's auto unwrap tool. Most 3D creation suites have one and their output can vary depending on how complicated your object is. For a simple crate like ours we will get fairly good results. For something more organic or for machines or architecture that are complicated, it might be hard to get a good result. At the `Smart UV Project` menu, we are going to use the default settings. Click `Ok` and it will unwrap your object.

5. `Smart UV Project` has mashed our object flat and flat and separated it into UV islands. A UV island is a piece of your object. For our purposes, the way Blender has separated out our object will work just fine. If we were making a more complicated object, we could separate out each piece, group them to make more sense, and even stitch some of the pieces back together. This step will be necessary later in the book, when we create a more complicated object.

6. To improve the quality of our map, we can use the `Pack Islands` option found in the `UVs` menu at the bottom of the `UV Layout` viewport. Selecting this option will resize all of your UV islands to take up as much space as possible without causing stretching.

7. Since we are working in a game engine, we will need a second UV channel, or set of coordinates, for lighting information. To add another UV channel, we need to head back to the `Scene Layout` dropdown and select `Default`:

   ![UV unwrapping our game asset](OEBPS/Images/BO5189_03_21.jpg)

   Creating our second UV map channel

   In the `Properties` panel, click on the icon for `Object Data`. Under the `UV Maps` heading, click the plus button to create another UV channel.

8. On this second UV channel, we are going to add our lightmap UVs. These differ from our UV layout for texturing in that we don't care about making sure all the pieces are scaled to the same value. We only care that they take up as much space as possible to store light and shadow info. Blender's `Lightmap Pack` tool can easily do that for us. Head back to the `UV Layout` option in the `Scene Layout` drop-down menu. In the lower-right corner of the `UV Layout` viewport is the active UV channel. It will say `UVMap`. Click it and select `UVMap.001`. This is our new channel:

   ![UV unwrapping our game asset](OEBPS/Images/BO5189_03_22.jpg)

   Using the Lightmap Pack tool to create a lightmap that Unreal 4 can use

9. Head over to the `Tools` panel on the 3D viewport side and select `Lightmap Pack` from the `UV Mapping` menu. The default options are OK here as well. Click `Ok`. Now that we are done unwrapping. Head back to the `Default` screen layout:

   ![UV unwrapping our game asset](OEBPS/Images/BO5189_03_23.jpg)

   Adding the UV texture grid as a texture map to our crate

10. To test our UV layout, we can add the colored UV texture grid we downloaded earlier. This will easily show us if there is any stretching of our texture. If we were unwrapping this by hand, this texture map would help us put together a UV map and ensure all of our elements are facing one direction and minimize stretching. Click on the `Textures` button on the `Properties` panel toolbar. In the first section of the menu, go to the `Type` dropdown and select `Image or Movie`. This will open up the `Image` section of the menu and allow you to click the `Open` button. Navigate to where you stored your image and select it.

11. Click the icon in the `Properties` panel toolbar next to the `Texture` icon. This is the `Materials` panel. All that is left to do is to `Assign` the material to your object. Click the `Select` option to select all your polygons, then click `Assign`. This adds the material to our object. It is stretched around the object according to our UV map.

12. Obviously, we don't want a super colorful box in our Unreal environment. In the next section we will talk about adding our own basic material to the box and about other techniques for creating detailed textures.

## Basic Texturing Techniques

As I mentioned earlier, textures are what make a game object pop in your level. They can be used to add all kinds of different elements we would not take the time to model, as well as color, wear and tear, and lighting information. There are many ways to texture our game object. Basic objects that would like to take advantage of a material in Unreal can apply one in the level. Blender has painting tools available within the program through its texture paint mode. Lastly, there are options available using other programs to create your textures. Our UV map can be exported to a program like `Adobe Photoshop` or `GIMP`, where we can paint the textures using the color tools or use photos to create a realistic result. I also want to talk about a new type of tool that has been available for a couple of years now. `Substance Painter` is an application that allows you to paint textures directly on to your 3D object, much like Blender's texture painting mode, but it has options and tools built specifically for the digital entertainment industry:

![Basic texturing techniques](OEBPS/Images/BO5189_03_24.jpg)

Showing off our new crate within Unreal 4

Unreal comes with several materials available for our use through the `Starter Content` folder and since our UV map is going to import in with our object, it would be as simple as dragging and dropping the material on to our crate. The materials themselves would look great on the crate and would definitely provide the depth and character we would want for our level. However, since the materials are not made specifically for our crate they would not really provide some of the additional detail that we talked about earlier. When we import our object into Unreal in our next chapter, we will first try this route:

![Basic texturing techniques](OEBPS/Images/BO5189_03_25.jpg)

Blender's Texture Paint mode

Blender itself comes with a Texture Paint mode. This mode can be used to paint solid colors and images on to an object. The resulting texture can then be exported from the program and applied within Unreal. Using this mode allows us to create our texture for our crate without involving any other programs, but since Blender is not a dedicated graphics editing program, the tools it provides are limited:

![Basic texturing techniques](OEBPS/Images/BO5189_03_26.jpg)

Our exported UV map shown inside Adobe Photoshop

`Adobe Photoshop`, and its open source alternative `GIMP`, are graphics editing programs that can be used for many forms of digital drawing, photo editing, and more. By importing our UV map into one of these programs, we can use their drawing tools to create detailed textures that can be imported into Unreal along with our object. This has been the most common way of creating game textures for many years. `Adobe Photoshop` is available for a 30-day trial subscription at `http://www.adobe.com/products/photoshop.html`. `GIMP` can be downloaded at `http://www.gimp.org`:

![Basic texturing techniques](OEBPS/Images/BO5189_03_27.jpg)

Our crate inside Allegorithmic Substance Painter
