
## Other controls in Minecraft Pi

When a game is running, pressing the Tab key will take the focus away from the game, and the mouse cursor will be freed to enable interaction with other windows on the desktop. Pressing Esc will pause the game and will take us to the game menu, where we can toggle views (as seen earlier), enable/disable sound, and quit the main window.

> Now, with all this information on the movements and the actions in the Minecraft Pi virtual game world, try to create a few things before continuing with the remainder of this chapter.

## Python programming for Minecraft Pi

Minecraft Pi comes with the Python programming interface. This means that it's possible to script the action of a character in the game using Python. In this section, we will learn how to use Python programming to script the character actions as well as create amazing effects in the Minecraft world.

Start Minecraft Pi and create the world. Once it's done, while the game is running, free the mouse by pressing the Tab key and opening lxterminal. Create the /home/pi/book/chapter02 directory. Navigate to the directory and write the following code in the prog1.py file:

```
import mcpi.minecraft as minecraft

mc = minecraft.Minecraft.create()

mc.postToChat("Hello Minecraft World!")
```

The preceding program first imports the Minecraft Pi Python API. The second statement creates the connection to Minecraft, and the third statement posts the message to the game chat.

Run the preceding code; the following is its output:

![](img/631e8b200cce187082ee46b0eb1681b3_57_0.png)

Let's take a look at some of the most important functions of the Minecraft Pi Python API.

We can see the player's current coordinates in the game window in the top-left corner of the window. We can retrieve these coordinates with `getPos()`. The following is the code to retrieve the player character's current coordinates and print them:

```
```python
import mcpi.minecraft as minecraft
mc = minecraft.Minecraft.create()
cur_pos = mc.player.getPos()
print cur_pos.x ; print cur_pos.y ; print cur_pos.z
```
```

This will print the current coordinates to the screen. Alternatively, the following syntax can be used to achieve this:

```
```python
import mcpi.minecraft as minecraft
mc = minecraft.Minecraft.create()
cur_x , cur_y , cur_z = mc.player.getPos()
print cur_x ; print cur_y ; print cur_z
```
```

The setPos() function can be used to set the character's position. We need to pass the desired coordinates of the character to this function, as follows:

```
import mcpi.minecraft as minecraft
mc = minecraft.Minecraft.create()
cur_x , cur_y , cur_z = mc.player.getPos()
mc.player.setPos(cur_x+10, cur_y, cur_z)
```

The preceding code will displace the character by 10 blocks in the x axis.

The following code will set the character position 100 blocks higher than the current position, and as a result, the character will start falling:

```
mc.player.setPos(cur_x, cur_y + 100 , cur_z)
```

We can place the blocks of our choice with the setBlock() function. We need to pass the coordinates of the block (the first three arguments) and the type of the block (the fourth argument) we need to set to this function as follows:

```
import mcpi.minecraft as minecraft
import mcpi.block as block
mc = minecraft.Minecraft.create()
cur_x , cur_y , cur_z = mc.player.getPos()
mc.setBlock(cur_x + 1 , cur_y , cur_z , block.ICE.id )
```

You will find an ice block placed in front of you. If you do not find the block, then try to look around; it will be just beside or behind you.

Some blocks (such as wool and wood) have additional properties. We can pass this additional property as the fifth argument to the function for the wool and wood block types. The following code creates a column of a wool block with all the possible colors:

```
import mcpi.minecraft as minecraft
import mcpi.block as block
mc = minecraft.Minecraft.create()
cur_x , cur_y , cur_z = mc.player.getPos()

for i in range (0 , 15):
    mc.setBlock(cur_x + 1 , cur_y + i , cur_z , block.WOOL.id, i )
```

Run the preceding code and check the output for yourself.

We can use `setBlocks()` to place multiple blocks for a given volume. The following example places multiple gold blocks:

```
```python
import mcpi.minecraft as minecraft
import mcpi.block as block
mc = minecraft.Minecraft.create()
cur_x , cur_y , cur_z = mc.player.getPos()

mc.setBlocks(cur_x + 1 , cur_y + 1 , cur_z + 1 , cur_x + 6 ,
cur_y + 6 , cur_z + 6 , block.GOLD_BLOCK.id)
```
```

We can also use mathematical equations to draw geometric shapes in the game world. Now we know that `setBlock()` is used to place a single block. We can use this function in a single `for` loop to create a line of blocks. Calling this function in a double `for` loop will set a two-dimensional geometric shape. We can further extend this by introducing one more `for` loop. This will create a three-dimensional shape. The following code places a golden sphere near the player's position. We will place the gold block in the positions where the coordinates satisfy the equation of the sphere with a radius of 10 blocks:

```
```python
import mcpi.minecraft as minecraft
import mcpi.block as block

mc = minecraft.Minecraft.create()

r = 10

cur_x , cur_y , cur_z = mc.player.getPos()

for x in range(r*-1,r):
    for y in range(r*-1, r):
        for z in range(r*-1,r):
            if x**2 + y**2 + z**2 < r**2:
                mc.setBlock(cur_x + x, cur_y + ( y + 20 ) , cur_z - (
                z + 20 ) , block.GOLD_BLOCK)
```
```

## Minecraft Pi

The following is the output of the preceding program:

![](img/631e8b200cce187082ee46b0eb1681b3_60_0.png)

The following code places a gold block below the player's current position until the execution of the code is terminated by pressing **Ctrl + C**:

```
```python
import mcpi.minecraft as minecraft
import mcpi.block as block
import time

mc = minecraft.Minecraft.create()

while 1:
    cur_x, cur_y, cur_z = mc.player.getPos()
    mc.setBlock(cur_x,cur_y-1,cur_z,block.GOLD_BLOCK.id)
    time.sleep(0.1)
```
```

The following is the output of the preceding code:

![](img/631e8b200cce187082ee46b0eb1681b3_61_0.png)

Now, we will conclude the chapter with some explosions. We can place a TNT block near us. It can be activated by left-clicking while holding the sword in hand. It will explode in a few seconds. The explosion will destroy some blocks in the blast radius:

```
```python
cur_x, cur_y , cur_z = mc.player.getPos()
mc.setBlocks(cur_x+1,cur_y,cur_z,block.TNT.id,1)
```
```

This can be made more spectacular by placing multiple TNT blocks, as follows:

```
```python
import mcpi.minecraft as minecraft
import mcpi.block as block

mc = minecraft.Minecraft.create()

cur_x, cur_y , cur_z = mc.player.getPos()
mc.setBlocks(cur_x+1,cur_y,cur_z,cur_x+4,cur_y+3,
cur_z+3,block.TNT.id,1)
```
```

Once blocks are placed, activate the TNT block by the sword. The following is the screenshot of an explosion after activating the TNT blocks placed by the preceding code:

![](img/631e8b200cce187082ee46b0eb1681b3_62_0.png)

> The detailed Minecraft Pi Python API can be found at http://www.stuffaboutcode.com/p/minecraft-api-reference.html.

## Summary

In this chapter, we got ourselves familiarized with the Minecraft Pi gameplay and learned how to program it using the Python API. We learned how to place and remove a block. Also, we implemented the code required to create some wonderful geometric shapes. In the end, we learned how to activate and explode TNT blocks to blow the large areas in the in-game world.

In the next chapter, we will learn how to use the Pygame library in order to create a game for the Pi.

## Building Games with PyGame

In the previous chapter, we learned how to get started with Minecraft on Raspberry Pi and how to play it and use Python to manipulate things. Continuing with the same theme, we will now take a look at a gaming library in Python called PyGame and also learn how to create simple games with it. In this chapter, we will go through the following topics:

- Introducing PyGame
- Drawing a fractal tree
- Building a simple snake game

### Introducing PyGame

PyGame is a set of Python modules designed for the writing of video games. It is built on top of the existing **Simple DirectMedia (SDL)** library, and it works with multiple backends, such as OpenGL, DirectX, X11, and so on. It was built with the intention of making game programming easier and faster without getting into the low-level C code that was traditionally used to achieve good real-time performance. It is also very flexible and comes with many operating systems. It is very fast as it can use multiple core CPUs very easily and also use optimized C and assembly code for core functions.
