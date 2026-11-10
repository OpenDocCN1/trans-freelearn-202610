![](bbox=[0.26, 0.13, 0.73, 0.28])

Normal ball and a fractured one: each fractured element is a separate game object

You can also download the provided example Unity project and go to the `Chapter 09 Physics and animations\Recipe 03 Destructible objects` directory. You can find the `Example.unity` scene there. If you play the game, you will see a ball falling down. When it hits the ground, it will be fractured into pieces.

# How to do it...

To create a destructible object, follow these steps:

+   1. Import the normal and shattered objects to Unity. In our example, we have an object called **Ball** and the other one is called **BallFractured**.
+   2. Drag and drop both objects into the scene.
+   3. Select all child game objects (pieces) of the **BallFractured** game object. Add the **Rigidbody** and **Mesh Collider** components to them. Set the **Mesh Collider** to **Convex**.
+   4. Create a prefab from the **BallFractured** game object (containing all the pieces).
+   5. Select the **Ball** game object and add a **Rigidbody** and a **Sphere Collider** to it (or another type, depending on the shape of your object).
+   6. Write a new C# script and call it `SpawnFracturedObject.cs`. In this script, we have a `void Fracture()` function that spawns a fractured game object prefab (our **BallFractured**), applies velocity to all its pieces, and destroys the main object (**Ball**):

```
void Fracture()
{
    GetComponent<Collider>().enabled = false;
    GameObject fracturedObject =
        (GameObject)GameObject.Instantiate(fracturedObject
```