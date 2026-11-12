![](bbox=[0.126, 0.089, 0.865, 0.455])

Figure 8.24 – Collect the ammo pickup to increase your ammo counter

We've learned how to create a Blueprint for a pickup that modifies the player's status when it is collected. One of the final steps we need to carry out to establish a full game loop is to create a condition for the player to win. We will look at that next.

# Setting a gameplay win condition

We will modify our **HUD** Blueprint and player character Blueprint to account for a target goal that the player must strive to achieve. We will display the target goal in the HUD next to the target count, so the player can easily see how many targets need to be destroyed to reach their goal.

We will also create another Widget Blueprint representing a win menu screen that will be shown to the player when they reach their goal. Finally, we will implement the logic needed to check whether the player has won and to show the win menu screen.