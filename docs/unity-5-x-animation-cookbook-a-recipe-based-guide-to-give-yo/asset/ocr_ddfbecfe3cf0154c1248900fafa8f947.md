```
for (int i = 0; i < rigidbodies.Count; i++)
{
    rigidbodies[i].isKinematic = !enable;
}
if (mainCollider != null)
{
    mainCollider.enabled = !enable;
}
if (mainRigidbody != null)
{
    mainRigidbody.isKinematic = enable;
}
if (anim != null)
{
    anim.enabled = !enable;
}
```

+   10. We call this function every time player presses the space bar. At the start of the game, we collect all rigid bodies attached to character's rig children objects and store them in a global list. We pass that list to the `EnableRagdoll()` function every time we call it. We also call the `EnableRagdoll()` function at the start of the game with the `enable` parameter set to `false`. This disables the ragdoll.
+   11. Attach the script to the character and drag its rig to the **Characters Rig** field of the script.
+   12. Play the game and press the space bar to see the effect.

# How it works...

The **Create Ragdoll** window helps with attaching **Character Joint**, **Capsule Collider**, and **RigidBody** components to humanoid characters' rigs. Those characters don't have to use the HumanoidumanoidHumanoid rig in the same way as the Mecanim feature.

Rigid bodies, colliders, and character joint components create a ragdoll. To turn the ragdoll on, we need to disable the Animator component as animations override the movement of rigid bodies. This is what our script does. It also makes the ragdoll's rigid bodies kinematic when the ragdoll is off, to turn the physics simulation off when it's not needed.