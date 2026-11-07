3. Normally, the rope goes through a pully system. This allows the flag to be raised and lowered. To simulate this, let's duplicate the rope (*Shift + D*) so that there are now two ropes.

![](bbox=[0.32, 0.13, 0.68, 0.32])

Figure 9.13 – Rope position

Place the ropes in between the pole and the flag, as you can see in Figure 9.13, and then we can move on to creating the hooks!

# Creating the hooks

With real-life flags, depending on the size, there are usually two to four hooks that attach the flag to the rope. For this tutorial, we will be adding four hooks:

+   1. Press *Shift + A* and add a **Torus** object.
+   2. Scale it down to be around 0.03 m and place it in the top-left corner of the flag.

![](bbox=[0.26, 0.56, 0.74, 0.66])

Figure 9.14 – The hook position

3. Now, we need to make sure the torus object moves with the flag as it's simulating. To do this, we are going to parent the torus to the top-corner vertex of the flag. So, go into Edit Mode with the flag and torus objects both selected.