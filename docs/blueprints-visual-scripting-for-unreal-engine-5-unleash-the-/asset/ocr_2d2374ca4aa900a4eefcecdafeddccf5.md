+   10. Drag a wire from the **Key Name** pin of **Set Value as Vector** node and add a **GET Location Of Sound Key** node.

+   11. Connect the **Location** pin of the **Inputs** node to the **Vector Value** pin of **Set Value as Vector**, and connect the white execution pins of the **Inputs** and **Set Value as Vector** nodes.

+   12. The nodes of *steps 12–14* are to set the **True** value in the **Has Heard Sound Key** of the blackboard. Drag another wire from the **Return Value** pin of **Get Blackboard** and add a **Set Value as Bool** node. Check the **Bool Value** parameter.

+   13. Drag a wire from the **Key Name** pin of the **Set Value as Bool** node and add a **GET Has Heard Sound Key** node.

+   14. Connect the white execution pins of the **Set Value as Vector**, **Set Value as Bool**, and **Outputs** nodes. Close the **UpdateSoundBB** macro tab and compile the blueprint.

With the **UpdateSoundBB** macro created, we will use another event of the **PawnSensing** component to detect sound.

# # Interpreting and storing the noise Event data

The **PawnSensing** component we added to `BP_EnemyController` gives us the foundation to build both visual and auditory sensing in our enemy AI. We will use the **On Hear Noise** event that activates any time the **PawnSensing** component detects a special type of sound broadcast by a pawn noise emitter.

We will have to set up the blueprint so that the enemies only detect noises that are made a short distance away; otherwise, it would feel unfair for the player to shoot their gun from the opposite corner of the map and let every enemy instantly know their location.

Follow these steps to create the **On Hear Noise** event: