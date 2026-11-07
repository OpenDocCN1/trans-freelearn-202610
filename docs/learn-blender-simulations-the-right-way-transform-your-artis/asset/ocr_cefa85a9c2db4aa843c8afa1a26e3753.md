7. With the **Elevator Platform** object still selected, press N to open the **Properties** panel. Let’s lock all the **Rotation** and **Location** axes *except* for the **Z** location; this way, when we add the motor constraint, it will only move up along the **Z** axis.

![](bbox=[0.37, 0.16, 0.63, 0.36])

Figure 11.11 – Locked properties

8. Select both the **Button Cube** and **Elevator Platform** objects and head to **Object | Rigid Body | Connect**. This will create a constraint between these two objects.

![](bbox=[0.33, 0.45, 0.67, 0.73])

Figure 11.12 – An empty constraint

This constraint will have **X**, **Y**, and **Z** handles. Keep the rotation of these handles in mind because they determine the direction and rotation of a lot of the constraint types.