# There's more...

LookAt() aiming is best used with robots and machines. It can be very useful for implementing turrets. You can find a Turret game object in the scene. It is built of three objects:

+   - **Turret**: This is the base of the Turret.
+   - **TurretPivot**: This game object only rotates horizontally (in the Y axis). This is a child object of the Turret.
+   - **Gun**: This object always points at the target. It is a child object of the TurretPivot. The TurretPivot rotates horizontally, so the Gun looks like it only rotates vertically.

The Turret game object has a TurretAim.cs script attached. In this script's Update() function, we first damp the public Transform target position and save the value in the Vector3 dampedTargetPosition variable. Then we use this variable to calculate the aimVector and we use the aimVector to calculate the horizontalAimVector (we simply remove the Y component from it). Lastly, we calculate new rotations for the TurretPivot and Gun game objects (stored in public Transform variables) using our aimVector and horizontalAimVector:

```
dampedTargetPosition =
Vector3.SmoothDamp(dampedTargetPosition,
 aimTarget.position, ref refDampSpeed, dampTime);

 aimVector = dampedTargetPosition - turretGun.position;

horizontalAimVector = aimVector;
horizontalAimVector.y = 0f;

turretPivot.rotation =
Quaternion.LookRotation(horizontalAimVector);
turretGun.rotation = Quaternion.LookRotation(aimVector);
```

# Using Blend Trees to aim

In this recipe, we will use a more sophisticated method of aiming. This method is based on using several aim animations combined into a Blend Tree.