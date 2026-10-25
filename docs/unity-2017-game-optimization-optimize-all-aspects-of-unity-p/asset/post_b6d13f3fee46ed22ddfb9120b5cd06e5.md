

## Optimize Ragdolls

Speaking of Joint-based objects, Ragdolls are incredibly popular features for good reason; they’re tons of fun! Ignoring the morbidity of flinging corpses around a game world for the moment, there’s something about watching a complex chain of objects flail around and smash into things that hits a lot of psychological fun buttons. This makes it very tempting to allow many Ragdolls to coexist within our Scene at the same time, but as we quickly discover, this risks an enormous performance hit when too many Ragdolls are in motion and/or collide with other objects due to the amount of iterations the Solver would need to resolve them all. So, let’s explore some ways to improve the performance of Ragdolls.

### Reduce Joints and Colliders

Unity provides a simple Ragdoll-generation tool (the Ragdoll Wizard) under **GameObject | 3D Object | Ragdoll...**. This tool can be used to create Ragdolls from a given object by selecting the appropriate child `GameObjects` to attach `Joint` and `Collider` Components to for any given body part or limb. This tool always creates 13 different Colliders and associated Joints (pelvis, chest, head, two Colliders per arm, and three Colliders per leg).

Note that a bug causes the Ragdoll Wizard not to complain if nothing is assigned to **Left Foot** or **Right Foot** `Transform` Component references like it does for the rest of them, but Unity will throw a `NullReferenceException` if we try to create the mesh without them assigned. Ensure that all 13 `Transform` Component references have been assigned when we try to create a Ragdoll.

However, it’s possible to use only seven Colliders (pelvis, chest, head, and one Collider per limb) to greatly reduce the overhead cost at the expense of Ragdoll realism. This can be achieved by deleting unwanted Colliders and manually reassigning the Character Joint’s `connectedBody` properties to the proper parent joints (connect the arm Colliders to the chest, and connect the leg Colliders to the pelvis).

Note that we assign a `mass` value during Ragdoll creation using the Ragdoll Wizard. This mass value is spread across the various Joints as appropriate and, therefore, represents the total mass of the object. We should ensure that we don't apply a mass value too high or too low compared to other objects in our game to avoid potential instability.

### Avoid inter-Ragdoll collisions

The performance cost of Ragdolls grows exponentially when Ragdolls are allowed to collide with other Ragdolls, since any Joint collision requires the Solver to calculate the resultant velocity applied to all of the Joints connected to it and then each of the Joints connected to them, such that both Ragdolls must be completely resolved multiple times. Also, it gets significantly more complicated if multiple parts of the Ragdolls are likely to collide with one another during the same collision.

This is a tough task for the Solver to handle, so we should avoid it. The best way to do this is to simply use the Collision Matrix. It is wise to assign all Ragdolls to their own Layer and uncheck the corresponding checkbox in the Collision Matrix so that objects in the given Layer cannot collide with objects in the same Layer.

### Replace, deactivate or remove inactive Ragdolls

In some games, once a Ragdoll has reached its final _destination_, we no longer need it to remain in the game world as an interactable object. We could then either deactivate, destroy, or replace the Ragdoll with a simpler alternative when they are no longer needed (a good trick is to replace them with the simpler version that uses only seven Joints as suggested earlier). Such simplifications are often implemented as a means of reducing overhead for weaker hardware/lower quality settings or as a compromise to allow more Ragdolls to coexist in our Scene. It could even be used dynamically if a particular number of Ragdolls is already present.

We would need some object to keep track of all of our Ragdolls, being notified any time a Ragdoll is created, keeping track of how many Ragdolls currently exist, watching each of them until they fall asleep through `RigidBody.IsSleeping()` and then do something appropriate with them. The same object could also choose to instantiate simpler Ragdoll variations if the Scene already contains more Ragdolls than is reasonable. This would be another good opportunity to make use of the Messaging System we explored in _Chapter 2, Scripting Strategies_.

Whichever approach we choose to improve the performance of our Ragdolls will no doubt result in limiting Ragdolls as a gameplay feature, either by instantiating fewer of them, giving them less complexity, or giving them a shorter lifetime, but these are reasonable compromises to make given the performance-saving opportunities.

### Know when to use physics

The most obvious method to improve the performance of a feature is to avoid using it as much as possible. For all moveable objects in our game, we should take a moment to ask ourselves if getting the Physics Engine involved is even necessary. If not, we should look for opportunities to replace them with something simpler and less costly.

Perhaps we're using physics to detect whether the player fell into a kill-zone (water, lava, a death-plummet, and so on), but our game is simple enough that we only have kill-zones at a specific height. In this case, we could avoid physics Colliders altogether and get away with only checking whether the player's y-position falls below a particular value.

Consider the following example--we're trying to simulate a meteor shower, and our first instinct was to have many falling objects that move via physics Rigidbodies, detect collisions with the ground via Colliders, and then generate an explosion at the point of impact. However, perhaps the ground is consistently flat or we have access to the Terrain's Height Map for some rudimentary collision detection. In this case, object travel could be simplified by manually tweening the objects' `transform.position` over time to simulate the same traveling behavior without requiring any Physics Components. In both cases, we can reduce the physics overhead by simplifying the situation and pushing the work into Script code.

> Tweening is a common short-hand term for in-betweening, which is the act of interpolating variables from one value to another gradually over time. There are many useful (and free) tweening libraries available on the Unity Asset Store that can provide a lot of useful functionality. Although, be careful of potentially poor optimization in these libraries.

The reverse is also possible. There might be occasions where we're performing a great deal of calculation through Script code that could be handled through physics relatively simply. For example, we might have implemented an inventory system with many objects that can be picked up. When the player hits the `Pick Up Object` key, each of these objects might be compared against the player's position to figure out which object is the closest.

We could consider replacing all of the Script code with a single `Physics.OverlapSphere()` call to get nearby objects when the key is pressed, and then figure out the closest pickup object from the result (or, better yet, just automatically pick up all of them. Why make the player repeatedly click more than necessary?). This could greatly reduce the total number of objects that must be compared each time the key is pressed, although comparisons should be made to ensure that this is the case.

Ensure that you seek to remove unnecessary physics grunt work from your Scenes or use physics to replace behavior that is costly when performed through Script code. The opportunities are as wide and far reaching as your own ingenuity. The ability to recognize opportunities like this takes experience, but is a vital skill that will serve you well when saving performance in current and future game development projects.

## Summary

We've covered numerous methods to improve our game's physics simulation both in terms of performance and consistency. The best technique when it comes to costly systems such as Physics Engines is simply avoidance. The less we need to use the system, the less we need to worry about it generating bottlenecks. In the worst-case scenario, we may need to reduce the scope of our game to condense physics activity down to only the essentials, but as we've learned, there are plenty of ways to reduce physics complexity without causing any noticeable gameplay effects.

In the next chapter, we will immerse ourselves in Unity's Rendering Pipeline and discover how to maximize the graphical fidelity of our application, by making use of all of the CPU cycles we've freed up using the performance enhancements from earlier chapters.

