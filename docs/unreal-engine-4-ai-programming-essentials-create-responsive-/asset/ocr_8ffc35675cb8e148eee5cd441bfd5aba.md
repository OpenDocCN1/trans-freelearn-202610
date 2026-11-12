+   11. By pulling from the **SpringArm** variable's current **RelativeRotation** pin, we want to create the **RInterp To** node.
+   12. Next, we want to get our **Direction** variable and convert this direction to
+   a rotation using **Make Rot from X**.
+   13. We will change this rotator by an offset. I calculated this as the local rotation
+   offset of the spring. With the direction in **A**, we should set **B** to have **0.0 Roll**,
+   **45.0 Pitch**, and **90.0 Yaw**.
+   14. Now, connect the **Return Value** pin to the **Target** pin of our **RInterp To** node.
+   15. Now, right-click, search for **Get World Delta Seconds**, and plug this into the
+   **Delta Time** pin on our **RInterp To** node.
+   16. The **Return Value** pin must now be plugged into **New Rotation** for the
+   **SetRelativeRotation** node we created earlier.

> So, we want to update our direction here. The reason for this is that
> we will continue to add to this operation to normalize our direction
> and get a value in the range we expect.

Comment this area **Calculate Direction for Camera Spring Arm**:

![](bbox=[0.18, 0.475, 0.82, 0.68])

+   17. So, to do this, let's pull our **Direction** variable down. Pull from the variable
+   and find Vector + Vector. From here, we want to normalize the vector. Lastly,
+   let's break the vector.