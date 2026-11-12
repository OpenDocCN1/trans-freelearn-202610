+   9. In the **Details** panel of the **Wait** node, set **Wait Time** to `3.0` to add a 3-second pause between patrols. Set **Random Deviation** to `1.0` to add a 1-second variation. This will result in a pause of random length between 2 and 4 seconds:

![](bbox=[0.127, 0.198, 0.762, 0.372])

Figure 9.41 – Adjusting the wait time

+   10. Save the Behavior Tree. When running the game, the Behavior Tree will execute the **Move To** task node until the enemy reaches its destination. When this happens, the **Wait** task node will be executed: