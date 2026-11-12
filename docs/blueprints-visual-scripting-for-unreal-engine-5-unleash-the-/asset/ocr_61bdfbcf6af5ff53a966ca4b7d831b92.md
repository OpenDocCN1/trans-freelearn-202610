As an example, let's create a Blueprint called BP_LightSwitch. The BP_LightSwitch Blueprint has an object reference variable of the Point Light type that references a Point Light placed in the Level. When the player overlaps the BP_LightSwitch Blueprint on the Level, it toggles the visibility of the Point Light.

To create the Blueprint, follow these steps:

+   1. Open any existing project that you created that contains starter content or create one if you want.

+   2. Click on the Content Drawer button to open Content Browser, then click the Add button and select Blueprint Class.

+   3. On the next screen, choose Actor as the parent class.

+   4. Name the Blueprint BP_LightSwitch and double-click it to open the Blueprint Editor.

+   5. Click the Add button in the Components panel and choose the Static Mesh Component. In the Details panel, choose the SM_CornerFrame Static Mesh, as shown in the following screenshot. This Static Mesh is a simple visual representation of our light switch. Also, change Collision Presets to OverlapAllDynamic so that Static Mesh will not block the player's movement: