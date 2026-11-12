# # Waypoints

In this chapter, waypoints represent the points that bots navigate to. We will make additional changes to our waypoints to limit who can traverse these points. This will be established by creating an array of actors on individual waypoints. When the pawns go to access the list of available waypoints, they will only add the waypoints they're allowed to. Here are the steps to perform for this:

+   1. Start in the same **Blueprint** folder as before. Right-click on it and create a new blueprint. Go to **Custom Classes** and select the **Target Point** actor. We will use this for the sprite it provides.
+   2. Name this new **Target Point** subclass `waypoint` and hit **Okay**.
+   3. Open our new `Waypoint` custom blueprint and create a new public **ThirdPersonCharacter** pawn array variable called `Allowed Access`. Then, select **Editable** so that we can modify this value directly from Unreal Editor:

![](bbox=[0.29, 0.4, 0.71, 0.62])

+   4. Now, place some waypoints in the world, and we want to try to make this random. This will allow our pawn's path to be more controlled when we go over some other topics later in the chapter.
+   5. I choose to place seven waypoints in my world and have them share about three of them to create some uniqueness in the paths. So, inside one of the nodes, let's add both of the available pawns that we placed in the world previously.