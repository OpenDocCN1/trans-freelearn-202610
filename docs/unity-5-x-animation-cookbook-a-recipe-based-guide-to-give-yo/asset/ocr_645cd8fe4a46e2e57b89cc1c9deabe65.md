The rest of the script handles selecting the body part we want to apply the force to. In this example, we are using the mouse input to cast a ray and try to hit a body part's collider. If we manage to do so, we choose this particular limb as the target that we want to apply the force to.

# There's more...

You can also use the concept illustrated in the *Destructible objects* recipe to store the velocity of the character (or of its limbs) and then apply it to the ragdoll's body parts. To do so, you may calculate the linear and angular velocity of any given body part in the `Update ()` function, by subtracting the previous position from the current position, and the previous rotation in Euler angles from the current rotation. Then you can apply the calculated values as the `velocity` and `angularVelocity` to that part's `Rigidbody` component when you turn the ragdoll on.

# Dismemberment

This recipe shows how to create a dismemberment effect for a rigged character. We are going to create a decapitation as an example. This effect is often used in various combat games (both shooters and melee combat games):

![](bbox=[0.22, 0.58, 0.78, 0.81])

Decapitation—an example of the dismemberment effect