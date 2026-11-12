18. Next, let's pull the **Direction** variable and create a set variable. Let's split the **Direction** set variable using **Break Vector**. Then, plug both **X** and **Y** float variables into the **Direction X** and **Direction Y** variables of the SET node. Comment this area **Calculate Direction**:

![](bbox=[0.18, 0.225, 0.82, 0.435])

19. We need to pull a **Ball** variable and call a node named **Add Torque**. 20. To calculate the **Torque** value, we want to take the **Direction** variable. Get the **Roll Torque** variable and multiply it by the **Direction** vector. The results of this should be plugged into the **Torque** pin of the **Add Torque** node. Comment this area **Apply Torque to Ball**:

![](bbox=[0.18, 0.56, 0.82, 0.79])