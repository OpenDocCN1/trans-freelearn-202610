![](bbox=[0.126, 0.088, 0.866, 0.276])

# Figure 4.19 – Actions of Event ActorBeginOverlap

# Important Note

The **Get Game Mode** function returns a reference to **Game Mode** used by the current Level. But the type of the return value is **Game Mode Base**. By using a variable of this type, we were unable to access the **Add Game Score** function. Therefore, it was necessary to use **Cast To BP_GameModeWithScore**.

+   18. Compile `BP_Collectable`. In the Level Editor, drag and drop some instances of `BP_Collectable` in the Level. Click the **Play** button to test the Level. Use your character to collect the statues and see your current score printed on the screen.

# Important Note

The two white connection pins that appear in the previous screenshot are called **reroute nodes**. They can be added from **Context Menu** and are used to aid in the organization of the Blueprint.

This practical example shows two common ways of using the **Cast To** node. One way is to test whether an instance is of a specific type. The other way is to have access to variables and functions of a subclass. Now that we know how to use **Cast To**, let's learn how to add references and events of Actors in the Level Blueprint.