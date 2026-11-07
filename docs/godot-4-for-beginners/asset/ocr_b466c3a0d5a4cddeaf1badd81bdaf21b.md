+   - **_process**: The _process function is like the *heartbeat* of your game object. It runs repeatedly, once for every frame (like every snapshot of your game). You use it to update things continuously, such as moving a character or keeping track of the time. The delta inside the function is a small value that tells you how much time has passed since the last frame, so you can make sure everything happens smoothly, no matter how fast or slow the computer runs the game.

Now that you understand how functions work, you can take on board the concept of the game loop.

# Understanding the game loop

Most games run inside a loop that keeps updating what’s happening in the game—such as moving characters, checking for collisions, and drawing graphics to the screen. This is called the **game loop**. It runs many times per second (often 60 times per second or more), and every time it runs, it updates and redraws the game.

In Godot, you don’t usually write the loop yourself. Instead, Godot handles it for you and gives you special functions you can use to plug into the game loop. The three most common ones are discussed in the subsequent sections.

# _ready()

This function is called once when the node and its children are added to the scene tree and are ready to go. It’s like setting up your LEGO model before the game starts.

Use _ready() to do the following:

+   - Set initial values
+   - Connect signals
+   - Hide or show things at the start
+   - Prepare anything that only needs to happen once

# _process(delta)

This function runs every frame. It’s used for things that should happen constantly such as checking input, changing animations, or moving something smoothly.

Use _process(delta) for the following:

+   - Player input
+   - Animations