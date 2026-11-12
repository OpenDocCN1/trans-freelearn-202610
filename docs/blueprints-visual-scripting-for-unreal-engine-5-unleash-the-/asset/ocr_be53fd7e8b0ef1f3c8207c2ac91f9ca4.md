+   7. Drag a wire from the **Return Value** pin of **Load Game from Slot** and add a **Cast To BP_SaveInfo** node. Connect the white output pin of **Load Game from Slot** to the white input pin of the **Cast To BP_SaveInfo** node.

+   8. Connect the **Cast Failed** pin of the **Cast To BP_SaveInfo** node to the **Out** pin of the **Outputs** node. Add a reroute node if needed.

+   9. Drag a wire from **As BP Save Info** of **Cast To BP_SaveInfo** and add a **SET Save Info Ref** node. Connect the white pins of **Cast To BP_SaveInfo** and **SET Save Info Ref**.

+   10. Drag a wire from the output blue pin of **SET Save Info Ref** and add a **Get Round** node.

+   11. Drag a wire from the **Round** pin and add a **SET Current Round** node. We are getting the value of the **Round** variable that was saved and storing it in the **Current Round** variable of the `FirstPersonCharacter` Blueprint.

+   12. Connect the white pins of **SET Save Info Ref**, **SET Current Round**, and **Outputs** nodes.

+   13. Compile and save the Blueprint.

Using the **SaveRound** and **LoadRound** macros, we can make the player resume the game on the round they stopped. To increase the challenge, let's set the **TargetGoal** based on the round.

# # Increasing the TargetGoal

We will take advantage of the data we store in the save file to change the gameplay for the player as they progress. We will do this by creating a **RoundScaleMultiplier** variable that will multiply the **Current Round** to find the new **TargetGoal**.

Follow these steps to increase the **TargetGoal**:

+   1. In the **Variables** category of the **My Blueprint** panel, click on the + button to add a variable. In the **Details** panel, name it