+   6. Use the **HeadMarker** and **NeckMarker** to position the **Head** and the **Neck** to match the head and neck of the character as closely as possible. Remember that **Head**'s and **Neck**'s local position and rotation is 0, so when we position them by manipulating **HeadMarker** and **NeckMarker**, it will make is easier for us to spawn the **Head** and **Neck** later.
+   7. Add a **Sphere Collider** and a **Rigidbody** component to the **Head** game object.
+   8. Make a prefab from the **Head** game object and another one from the **Neck** game object.
+   9. Delete **Head** and **Neck** from the scene, but make sure to leave **HeadMarker** and **NeckMarker** still parented to their corresponding bones in the character's rig.
+   10. Create a new C# script and name it `Dismemberment.cs`. In this script's `Update()` function, we check whether the player presses the space bar and we call the `void Decapitate()` function if the `wasSevered` flag is `false` (you can really only decapitate someone once):
+   11. The `void Decapitate()` function does the rest. First it checks if the `HandleRagdoll.cs` script was found (the reference to it is set in the `Start()` function). If it was found, we enable the ragdoll with it and disable the script. Next we remove the **Character Joint**, **Rigidbody**, and **Collider** components from the head bone (we are going to scale it to 0 because we don't want to bother with this joint anymore). Next we spawn the new, severed **Head** from a prefab stored in the `public GameObject headPrefab` variable. The **Head** is spawned with the **HeadMarker** position and rotation, a reference to which is stored in the `public Transform headMarker` variable. Then we get the **Rigidbody** component of the spawned **Head** and apply a random force and random torque to make it move. We use the `public Vector3` variables to store the `randomForce1`, `randomForce2`, `randomTorque1`, and `randomTorque2` values. We have a small helper function `Vector3 RandomVector(Vector3 v1, Vector3 v2)` that takes two vectors and returns a random vector build from those two parameters. Lastly, we scale the head bone to 0 (reference to it is stored in the `public Transform head` variable). Finally, we spawn the **Neck** prefab and parent it to the **NeckMarker**:

```
if (Input.GetKeyDown(KeyCode.Space) && !wasSevered)
{
    Decapitate();
}
```

```
wasSevered = true;
if (ragdollHandler != null)
```