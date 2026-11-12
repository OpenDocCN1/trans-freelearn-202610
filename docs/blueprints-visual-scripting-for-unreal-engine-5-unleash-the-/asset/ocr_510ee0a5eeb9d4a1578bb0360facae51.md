Points parameter.

+   8. Add a **Branch** node to check whether the Actor has leveled up. Add the **Print String** and **Append** nodes to the EventGraph and connect them as shown in *Figure 18.36*. The conversion node is created automatically when you connect pins of different types.

+   9. In the **A** pin of the **Append** node, type `Leveled up` to with a space at the end. This node will create a `String` using the values of the **A** and **B** pins.

+   10. Compile the Blueprint and click the **Play** button of the Level Editor. A message will be printed on screen every time `ThirdPersonCharacter` levels up:

![](bbox=[0.125, 0.375, 0.335, 0.445])

Figure 18.37 – Printing a message on screen when the character levels up

If you need to display the experience points and the level of the character on screen, you can get the values by using the **Get Current XP** and **Get Current Level** nodes of the **BP Exp Level Comp** component:

![](bbox=[0.125, 0.59, 0.765, 0.75])

Figure 18.38 – Getting the Current XP and Current Level variables

We've learned how to create an Actor Component. Our next step is to create a Scene Component to attach other components to it.