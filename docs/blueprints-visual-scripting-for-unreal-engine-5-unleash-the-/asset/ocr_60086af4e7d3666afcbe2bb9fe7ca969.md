about from the win screen we created in _Chapter 8, Creating Constraints and Gameplay Objectives_, and apply it to a lose screen. This screen will enable the player to restart the level with full ammo and a freshly filled health bar, but will also negate any progress they had made toward reaching their target goal.

# # Setting up a lose screen

The lose screen will be presented when the player runs out of health. We will present them with options to restart the last round or quit the game. You may remember the win screen we created; we presented similar options there. Rather than remaking the UI screen from scratch, we can save some time by using our **WinMenu** asset as a template.

Follow these steps to create **LoseMenu**:

+   1. In the **Content Browser**, access the `Content|FirstPersonBP|UI` folder. Right-click on **WinMenu** and select the **Duplicate** option.
+   2. Name this new Blueprint Widget `LoseMenu`: