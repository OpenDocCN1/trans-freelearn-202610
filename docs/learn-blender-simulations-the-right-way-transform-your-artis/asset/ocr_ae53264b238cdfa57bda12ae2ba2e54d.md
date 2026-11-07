6. The top **Image** color socket in the **Alpha Over** node now controls the color of the background. Let’s set it to a nice dark gray color!

Now, the moment you have been waiting for… let’s add in vector blur!

7. Press **Shift + A**, go to **Filter | Blur**, and select **Vector Blur**. Place it in between the **Render Layers** and the **Alpha Over** nodes.

8. Take the **Vector** output from the **Render Layers** node and plug it into the **Speed** input. This will give us the information to add motion blur! Sometimes when using vector blur, you will get some bleed in areas of the image. This is because we don’t have **Depth** information yet. So, take the **Depth** socket and plug it into the **Z** input. Now, it should look much better. View the following screenshot:

![](bbox=[0.25, 0.32, 0.75, 0.58])

Figure 4.27 – Vector Blur

9. The **Samples** setting relates to the quality of the blur. Higher values will give better quality blur at the cost of render time. A value of 32 will be perfect for this scene.

10. **Blur** controls how much blur is there in the scene. A value of 0.25 might be a bit much, so let’s bring it down to 0.15.

11. One final thing before we render—make sure to turn off the visibility of the inflow object, or you will get a white plane in the render that won’t look very good. You can do this by toggling the **Eye** icon and **Camera** icon in the **Outliner**.