![](bbox=[0.125, 0.09, 0.865, 0.215])

Figure 18.31 – The first part of the IncreaseExperience function

+   16. The next screenshot shows the second part of the IncreaseExperience function. If the Yes pin of the Xp Reaches New Level macro is executed, then we increment the Current Level variable by one and set the Level Up Var variable to True. Add the nodes of the screenshot and compile the Blueprint:

![](bbox=[0.125, 0.405, 0.865, 0.605])

Figure 18.32 – The second part of the IncreaseExperience function

+   17. Compile the Blueprint and close the Blueprint Editor.

We have finished the BP_ExpLevelComp component. To test it, we need to add the component to an Actor.

# Testing the Actor Component

We will add the BP_ExpLevelComp component to ThirdPersonCharacter and use a timer to increase the experience points every second. We will print a message on screen every time ThirdPersonCharacter levels up.