UI will now do its job of responding to events that occur within our game. However, we still need to create the events that will trigger changes in the variables we have connected. In the next section, we will modify the player variables based on Actions that the player takes while playing.

# Tracking the ammo and targets eliminated

To get our UI to respond to the player interacting with the environment, we need to modify the player and target Blueprints. Let's start with the ammo counter to decrease when the player fires a shot from their gun.

# Reducing the ammo counter

We need to modify the player fire logic so that the ammo counter is decreased when the player fires their gun. Follow these steps:

+   1. In the Content Browser, access the /Content/FirstPersonBP/Blueprints folder and double-click on the FirstPersonCharacter Blueprint.

+   2. Find the large series of Blueprint nodes contained within the Spawn projectile comment block. We want to ensure that the counter tracking the player's current ammo count reduces by one each time the player fires a shot. The Blueprint scripting required to do so looks like this: