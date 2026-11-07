The components in Figure 9.7 will be used to make the cannon functional. It is a static body because it will not move, but the player can jump onto it and stand on it. The cannonball timer will regularly spawn cannonballs, which move across the level. The marker is used to pinpoint the position at which the cannonballs should appear.

We want the cannon to shoot a ball every three seconds. To do this, adjust the settings of the **Cannonball_timer** node, as shown in Figure 9.8:

![](bbox=[0.34, 0.24, 0.64, 0.42])

Figure 9.8 – Settings for the cannonball timer

With the settings shown in Figure 9.8, the cannon will automatically fire a ball every three seconds. Adjust **Wait Time** if you want to change the firing rate.

The cannon is now ready for scripting; however, we still need a ball or bullet for it to fire. We shall now prepare the **Ball** scene so that there is a projectile to launch from the cannon.

# Creating the Ball scene

Create a new scene and use a **CharacterBody3D** node as the root. Rename it to Ball.

It’s important to note that we would usually use an **Area3D** node for a projectile, since this allows us to detect collisions and trigger events such as applying damage or destroying the projectile on impact. However, in our game, the cannonball is meant to act only as a physical obstacle—it won’t damage the player but should simply push them if they come into contact.

Follow these steps to complete the Ball scene:

+   1. You have already created a new scene with **CharacterBody3D** node as the root.
+   2. You have renamed the root node to **Ball**.
+   3. Add a **MeshInstance3D** node as a child and use Bullet.obj as the Mesh property.
+   4. Apply palette.png as the texture of the bullet object.