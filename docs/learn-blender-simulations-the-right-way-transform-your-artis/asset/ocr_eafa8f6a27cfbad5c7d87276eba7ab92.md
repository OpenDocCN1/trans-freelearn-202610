4. Select the wrecking ball object. In real life, this would weigh a lot more, so let’s set **Mass** to 20 kg in the **Settings** tab. Also, change **Damping Translation** and **Rotation** to 0.400 in the **Dynamics** tab. This will make the wrecking ball stop swinging as the animation plays.

![](bbox=[0.25, 0.135, 0.75, 0.205])

Figure 11.3 – Damping values

5. Then, select the **Chain Top** object and turn off **Dynamic**. This will make sure it stays floating and doesn’t fall.

![](bbox=[0.25, 0.27, 0.75, 0.39])

Figure 11.4 – Chain Top settings

Normally, the **Passive** type is used for platforms or bigger surface areas; however, **Active** with **Dynamic** turned off is better for more complicated meshes.

From here, you will be able to play the animation and the wrecking ball should swing! Next up, let’s create the domino effect!

# Creating the domino effect

Now that we have the wrecking ball swinging, let’s have it crash into one of the **Domino** objects, causing a chain reaction, just like dominos would in real life. We want the dominos to fall in a sequence and eventually end up hitting the **Button Cube** object. This is what triggers the elevator to move upward!