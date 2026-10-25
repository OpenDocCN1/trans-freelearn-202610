We can also obtain a list of targets within a finite distance of a fixed point in space using a `Physics.OverlapSphere()` check. This is typically used to implement area-of-effect gameplay features, such as grenade or fireball explosions. We can even cast entire objects forward in space using `Physics.SphereCast()` and `Physics.CapsuleCast()`. These methods are often used to simulate wide laser beams, or if we simply want to see what would be in the path of a moving character.

# # Debugging Physics

Physics bugs usually fall into two categories: an object pair collided/didn't collide when it shouldn't have/should have or the objects collided but something unexpected happened after the fact. The former case is generally easier to debug; it is often either due to mistakes in the Collision Matrix, incorrect Layers used in Raycasting, or object Colliders being the wrong size or shape. The latter case is often much more difficult to resolve because of three big problems:

+   - Determining which collided objects caused the issue
+   - Determining the conditions of the collision just prior to resolution
+   - Reproducing the collision

Any of these three pieces of information would make resolution much easier, but they can all be difficult to obtain in some circumstances.

The Profiler provides some measure of information in the **Physics** and **Physics (2D) Areas** (for 3D and 2D physics, respectively), which can be moderately useful. We can get a measure of how much CPU activity is being spent on all Rigidbodies and groups of Rigidbodies isolated to different types such as Dynamic Colliders, Static Colliders, Kinematic objects, Trigger Volumes, Constraints (used to simulate hinges and other connected physics objects), and Contacts. The **Physics 2D Area** contains a little more information such as the number of sleeping and active Rigidbodies and how long processing the Timestep took. The **Detailed Breakdown View** provides even more information in both cases. This information helps us keep an eye on Physics performance, but it doesn't tell us much about what went wrong in the event we find a bug in our physics behavior.