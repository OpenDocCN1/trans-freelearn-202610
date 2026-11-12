To allow our UI elements to pull data from our player variables, we need to revisit the HUD Widget Blueprint. To get our UI to update with player data, we will create a **binding**. Bindings give us the ability to tie variables or functions of a Blueprint to a Widget. Whenever the variable or function is updated, that change is reflected in the Widget automatically.

So, instead of manually updating both the player's health stats and our Widget every time the player takes damage (so that the health meter display changes), we can bind the meter to the `PlayerHealth` player variable. Then, only one value will need to be updated.

# # Creating bindings for health and stamina

To create the bindings of the `PlayerHealth` and `PlayerStamina` variables with the progress bars' UI, follow these steps:

+   1. In the Content Browser, access the `/Content/FirstPersonBP/UI` folder and double-click on the **HUD** Widget Blueprint.

+   2. In the **HUD** UMG Editor, find the **Hierarchy** panel and click on the **Health Bar** object nested underneath the **Player Stats Bars** object.

+   3. With **Health Bar** now selected, locate the **Percent** field in the **Progress** category of the **Details** panel. Click on the **Bind** button next to **Percent** and select **Create Binding**, as shown in the following screenshot:

![](bbox=[0.12, 0.7, 0.87, 0.88])