![](bbox=[0.125, 0.09, 0.735, 0.46])

Figure 11.6 – Creating the Round integer variable

+   5. Compile, save, and close the Blueprint editor.

That's all we need to do in the BP_SaveInfo Blueprint. The next step is to learn how to save and load using the BP_SaveInfo Blueprint.

# Saving game information

Now that we have a container for our saved data, we need to ensure that the data is stored somewhere on the player's machine and that it is retrieved when the player returns to the game. Like the rest of our gameplay settings, we will add this process to the FirstPersonCharacter Blueprint. We will create some variables and a macro to save game information.

Follow these steps to create the variables and macro:

+   1. In the Content Browser, access the Content|FirstPersonBP|Blueprints folder and double-click on the