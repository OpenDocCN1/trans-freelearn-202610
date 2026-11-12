The structure has two variables. **VariantSet** is a reference to a **Variant Set** and **currentIndex** is an integer variable used to store the index of the selected Variant in a Variant Set.

Open the `BP_Configurator` Blueprint, located in the `Content > ProductConfig > Blueprints` folder. Let's start by looking at the variables in the **My Blueprint** panel:

![](bbox=[0.125, 0.26, 0.662, 0.519])

Figure 20.10 – The BP_Configurator variables

This is what each variable does:

+   • **ObjectVariantSets**: An array of **STRUCT Var Set** that stores all Variant Sets of the product
+   • **EnviroVarSet**: A Variable of the **STRUCT Var Set** type that stores the Variant Set of environment lighting
+   • **CameraVarSet**: A Variable of the **STRUCT Var Set** type that stores the camera Variant Set
+   • **LVSActor**: A reference to **Level Variant Sets Actor**
+   • **CameraActor**: A reference to the camera used to visualize the product
+   • **GUI**: A reference to the main UMG Widget BlueprintNoteThere are several variables in the **UI Options** category that can be used to modify