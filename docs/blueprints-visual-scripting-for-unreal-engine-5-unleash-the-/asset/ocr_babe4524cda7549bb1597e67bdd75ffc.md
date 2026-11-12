# Creating collectible objects

Restricting the player from firing their gun when they run out of ammo forces the player to be considerate of the accuracy of the shots they attempt within the game. However, limiting ammo would be unduly punishing without a way of acquiring more. We don't want ammo to naturally recharge like our stamina meter. Instead, we'll create a collectible ammo pickup to allow the player to regain ammo by exploring and traversing the level.

To create the BP_AmmoPickup Blueprint, follow these steps:

+   1. In the Content Browser, access the /Content/FirstPersonBP /Blueprints folder. Click the Add button and select Blueprint Class.

+   2. On the next screen, choose Actor as the parent class.

+   3. Name the Blueprint BP_AmmoPickup and double-click it to open the Blueprint Editor.

+   4. Click the Add button in the Components panel and choose the Static Mesh component. In the Details panel, choose the Shape_Pipe Static Mesh and in MATERIALS, go to Element 0 and choose M_Door. Change the X, Y, and Z values of the Scale attribute to 0.5, as shown in the following screenshot: