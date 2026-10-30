Test your installation by opening a Python terminal by entering python in a regular terminal and pressing *Enter*. Now, execute the following command:

```
import pygame
```

Now that you have your system set up and you have hopefully checked out the PyGame website to explore its complete functionalities, we will move on to build the binary fractal tree to introduce you to the workings of PyGame. Let's begin!

# # Drawing a binary fractal tree

A binary fractal tree is defined recursively by binary branching. Typically, it consists of a trunk of length 1, which splits into two branches of decreasing or equal length, each of which makes an angle Q with the direction of the trunk. Furthermore, both of these branches are divided into two branches, each making an angle Q with the direction of its parent branch, and so on. Continuing in this way, we can infinitely make branches, and the collective diagram is called a **fractal tree**. The following diagram visually shows what such a fractal tree might look like:

![](bbox=[0.273, 0.443, 0.729, 0.794])