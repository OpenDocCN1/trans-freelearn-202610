![](bbox=[0.125, 0.091, 0.865, 0.35])

Figure 9.22 – Changing the mesh material

+   5. In the **Components** panel, click on **CapsuleComponent** (**CollisionCylinder**) (**Inherited**). In the **Details** panel, change **Collision Presets** to **BlockAllDynamic**, and in the **RENDERING** category, check the **Hidden in Game** property:

![](bbox=[0.125, 0.52, 0.638, 0.688])

Figure 9.23 – Hiding CapsuleComponent

+   6. Compile the blueprint and drag the `BP_EnemyCharacter` blueprint onto the level to create an instance of the enemy in our play area.

In this section, we learned how to import assets from the Marketplace. We expanded the level and made it traversable using a NavMesh. We created the AI assets, and now we are ready to implement the navigation behavior of the enemy.