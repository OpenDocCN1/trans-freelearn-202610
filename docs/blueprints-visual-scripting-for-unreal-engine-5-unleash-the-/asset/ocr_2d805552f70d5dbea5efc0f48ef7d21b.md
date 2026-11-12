![](bbox=[0.126, 0.089, 0.868, 0.369])

# Figure 5.20 – Adding the Cast To node

FirstPersonProjectile is a Blueprint included in Unreal Engine 5’s **First Person** template, which controls the behavior of the projectiles that are fired from your gun. The `Cast` node is used to ensure that the Action attached to the execution pin of this node occurs only if the Actor hitting the Cylinder target is an instance of `FirstPersonProjectile`.

When the node appears, you should already see a blue wire between the **Other** output pin of the **Event Hit** node and the **Object** pin of the casting node. Connect the white execution pin of **Event Hit** to the execution pin of the **Cast To FirstPersonProjectile** node, and connect the output execution pin of the **Cast To FirstPersonProjectile** node to the execution pin of the **Set Material** node: