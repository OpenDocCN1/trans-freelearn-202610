# # Spawning more enemies during gameplay

We are going to spawn new enemies in the level periodically so that the game can continue if the player destroys the first few enemies, and if they are too slow to defeat enemies, then the difficulty will gradually increase.

# ## Creating the BP_EnemySpawner blueprint

We will create a blueprint that will spawn enemies in random locations in the level. The time between spawns is determined by a variable called `SpawnTime`. There is another variable called `MaxEnemies` that limits the spawning of enemies.

Follow these steps to create the blueprint:

+   1. In the content browser, access the `Content > FirstPersonBP > Enemy` folder. Click the **Add** button and choose the **Blueprint Class** option.

+   2. On the next screen, choose **Actor** as the parent class. Name the blueprint `BP_EnemySpawner` and double-click it to open the Blueprint Editor.

+   3. In the **Variables** category of the **My Blueprint** panel, click the **+** button to add a variable, and name it `SpawnTime`. In the **Details** panel, change **Variable Type** to **Float**, and check the **Instance Editable** attribute. Compile the blueprint and set **DEFAULT VALUE** to `10.0`: