15. We will do similar steps for the **Quit** button. Return to the **Designer** view and click on the **Btn quit** object, scroll down to the bottom of the **Details** panel, and click on the + button next to the **On Clicked** event to add an event.

16. You will be taken to the **Graph** view, where an **On Clicked (Btnquit)** node will appear. Drag from the output pin of **On Clicked (Btnquit)** onto **Graph** and add a **Quit Game** node so that the player can shut down the game by clicking the **Quit** button:

![](bbox=[0.126, 0.293, 0.866, 0.524])

Figure 8.39 – The action of the Quit button

17. Compile, save, and close the UMG editor.

Now that our **WinMenu** has been created, we need to tell the game when to show it to the player.

# # Displaying the WinMenu

As we did with the **HUD** Widget Blueprint, we will display the **WinMenu** from within the **FirstPersonCharacter** Blueprint. We will create a custom event named **End Game** that will be called when the game ends.

Follow these steps: