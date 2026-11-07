Finally, if you set **Bending Model** to **Linear**, the **Tension** and **Compression** settings will disappear and be replaced with **Structure**. This setting controls both the structure of the cloth and how much it will stretch.

# Damping

The **Damping** tab has all the same settings listed in the **Stiffness** menu: **Tension**, **Compression**, **Shear**, and **Bending**. These values basically slow down the effect of each setting in the **Stiffness** tab.

For example, if the **Compression** option in the **Stiffness** tab is set to 0 but **Compression** in the **Damping** tab is set to 50, the cloth will still collapse but it's going to take longer. If the **Compression** option in the **Damping** tab is also set to 0, then the cloth would collapse instantly and lose all its structure. So, again, these settings slow and dampen the effect of each option in the **Stiffness** tab.

# Internal Springs

Checking the **Internal Springs** box will basically turn your cloth into a soft body.

Remember that earlier in this chapter, we talked about springs for a cloth simulation? Well, normally, springs can only be applied to 2D surfaces, but when **Internal Springs** is enabled, 3D springs will be created inside the object, and now, it will behave very similarly to a soft body.

When you check the **Internal Springs** box, these options will be available:

![](bbox=[0.14, 0.5, 0.82, 0.77])

Figure 8.14 – Internal Springs settings