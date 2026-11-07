
With that out of the way, let’s create this fiery material:

1. Let’s start by selecting the **Shading** workspace at the top of the screen. The following figure shows where you’ll be able to create the fire material:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_89_0.png)

Figure 3.10 – The Shading workspace

2. To create a new material, click the big **New** button in the middle of the screen:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_89_1.png)

Figure 3.11 – New material

3. We don’t need the **Principled BSDF** shader, so go ahead and delete it. Then, replace it with the **Principled Volume** shader. To add a new node, press *Shift + A*, then go to **Shader** and select **Principled Volume**. From there, take the **Volume** output and plug it into **Volume** in the **Material Output** node, as follows:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_89_2.png)

Figure 3.12 – Principled Volume

4. Set **Density** to a value of 5 or so to make the smoke denser.

5. In *Chapter 1*, we used **Blackbody Intensity** to add flames. While this does work, using this method will produce some sharp edges and give the fire a lower-quality look. Instead, we're going to be using the **Heat** attribute to add the flames.

## Attributes

**Heat**, along with many other attributes, can be brought into the material and used however you like. For example, you could take the velocity attribute and plug that into the emission value. Then, you'll be able to see the values of the velocity in the simulation. *Flame, density, temperature,* and *velocity* are just a few of the attributes at your disposal. Also, the **Heat** attribute only works in the Cycles render engine.

For this to work, we need to add the **Attribute** node. To do this, press *Shift + A* and choose **Input | Attribute**.

6. In the **Name** field, type the word *heat* in lowercase. Then, plug the **Fac** output into **Emission Strength**.

7. To control this a bit more, let's add a **Color Ramp** node. To do this, click *Shift + A*, go to **Converter**, then select **Color Ramp** and place it in between the **Attribute** node and **Principled Volume**. If you slide the black handle of **Color Ramp** closer to the white handle, it will clamp down on those values, giving you the look of flames. You can also switch **Color Ramp** from **Linear** to **Ease** to get a smoother transition.

If you go into **Rendered View**, you'll see what the node setup should look like so far:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_90_0.png)

Figure 3.13 – Heat attribute

8. Let's brighten up our flames, shall we? To do this, add a **Math** node. Press *Shift + A*, go to **Converter**, and select **Math**. Then, place the node between **Color Ramp** and **Principled Volume**. If you set the mode from **Add** to **Multiply**, the bottom value will now control the strength of the emission. Let's set this to a value of 50.

9. Now, add some color. Duplicate Color Ramp by pressing Ctrl + Shift + D; this will keep the connection to the Attribute node (pressing just Shift + D will break the connection). Then, plug the new Color Ramp node into Emission Color in Principled Volume.

If we look at real flames, you'll notice that the edges have this red tint, and as you travel closer to the bottom of the fire, it goes orange and then white:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_91_0.png)

Figure 3.14 – Real flame example

To get the same look, select the black handle of the Color Ramp node and set it to a red color, then set the white handle to an orange color. Play around with the positions until it looks good. You can always change the colors later to get different looks for the fire!

10. Finally, to give our flames more detail, let's jump back to the first Color Ramp node and add another handle by clicking the + button. Move the handle we just added to the far right-hand side and set the color to black. Double-check that the white handle is a little bit to the left. View the following figure for an example:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_91_1.png)

Figure 3.15 – Fire material example

Remember that with the **Color Ramp** node moving, the black handles around it clamp down on the fire values. With this in mind, adding a black handle to the right-hand side removes some of the fire at the very bottom. This, in turn, gives a much more detailed and interesting look to the fire. View *Figure 3.15*.

And there you have it! We've finished creating the fire material. And just in case you got confused in this section, the following figure provides a full preview of all of the nodes:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_92_0.png)

Figure 3.16 – Full material preview

In the next section, we'll learn how to add sparks to our flames using a particle system!

## Creating sparks using a particle system

Now that the simulation is complete with a material, it's time to work on creating sparks that fly out of the fire. This is very easily done using a particle system. Using particle systems in Blender can be very useful and save a lot of time compared to adding the objects manually. The system works by emitting particles from a mesh object, normally in a great quantity. Things such as sparks, dust, hair, grass, and many more can easily be created using particles!

Now, let's create the sparks:

- 1. First, we need to add a new mesh object for the particles to emit from. For this scene, a circle would work great. Let's add one by pressing *Shift + A* and then going to **Mesh** and selecting **Circle**.
- 2. Once you've added the circle, look to the bottom left of the screen; you should see an **Add Circle** menu. Open it; this is where we set the properties for the circle. We need to make sure this mesh has a face and is small enough for the fire. Set **Fill Type** to **N-Gon** and **Radius** to 0.25. Then, place it underneath the fire:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_93_0.png)

Figure 3.17 – Adding a circle

3. Head over to the Particle System panel and create a new particle system by hitting the + button:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_93_1.png)

Figure 3.18 – Creating the particle system

4. We don't need 1,000 sparks, so let's set Number to 50. Feel free to experiment with this value until you find something you like!

5. Since the animation is only 100 frames long, set End Frame to 150 as well.

6. If you play our animation, you'll notice that large white dots are appearing. These are called halos. These halos can be used in many ways, including emitting fire/smoke from each particle, something we'll learn about in Chapter 5. For this tutorial, we need to use an actual object instead. Let's add Icosphere, move it to the left of the campfire so it's out of the way, and scale it down to 0.5.

7. Now select the Circle object again. To assign each particle as Icosphere, open the Render tab and set Render As to Object. Then, for Instance Object, select the Icosphere object we just added.

8. Play around with the Scale value until the particles are small enough. You can also increase Random Scale to give a random size to each particle.

9. We don't want this circle to show up in the render, so turn off Show Emitter:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_94_0.png)

10. Currently, the particles are falling, which isn't very good. Let's open Field Weights and set Gravity to 0:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_94_1.png)

11. The next question is, how do we make the particles follow the flow of the fire? Well, there's a force field that does that exact thing! Let's add it by pressing Shift + A and then going to Force Field and selecting Fluid Flow.

With Force Field selected, we need to jump over to the Physics panel and set Domain Object as the domain in our scene, which in this case is the Cube object. Now, the particles will move with the flow of the fire! If you want the particles to move faster, you can increase the Strength value:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_95_0.png)

Figure 3.21 – Fluid Flow settings

With that, we’ve finished creating the particle system! Feel free to play around with the number of particles, the strength of the force field, and whatever else you like to get your desired look! Before we talk about rendering, we need to create a material for our sparks.

## Creating the particle material

The next thing on the list is creating the particle material. In real life, right when sparks are emitted from a fire, they’re very bright white. As they rise and cool off, they turn to more of an orange-red-ish color. We can create this same look by adding a couple of different nodes to the material:

- 1. Select **Icosphere** and open **Shader Editor** once again. Then, create a new material by clicking the **New** button.
- 2. Delete **Principled BSDF** and replace it with **Emission Shader**, then set **Strength** to a value of 20.
- 3. Plug the **Emission** output into **Surface** in **Material Output**.
- 4. Next, we’re going to add a new node called **Particle Info**. To add this new node, press *Shift + A*, go to **Input**, and select **Particle Info**.

> **The Particle Info node**
> This node allows us to take certain data from the particle system and have it influence the material. For example, you could take the **Random** output and plug that into **Color Ramp** Now, each particle will have a random color based on **Color Ramp** One thing to note is that this node only works in the **Cycles** render engine.

5. We need to take the **Age** and **Lifetime** outputs from the **Particle Info** node and divide them together using a **Math** node. This will allow the color to change as the particle ages and rises into the air. To do this, press *Shift + A* and go to **Converter | Math**. Set the **Math** node to **Divide** and plug **Age** and **Lifetime** into the two inputs of the **Divide** node. Refer to Figure 3.22 if you get confused.
