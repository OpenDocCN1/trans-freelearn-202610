
PyGame was built to replace PySDL after its development was discontinued. Originally written by Pete Shinners, it is a community project from 2004 and is released under the open source free software GNU's lesser general public license. Since it is very simple to use and is open source, it has a lot of members in the international community and so it enjoys access to a lot of resources that other libraries may lack. There are many tutorials that can build different games with PyGame. It contains the following modules

| Module | The description |
|---|---|
| cdrom | Manages CD-ROM devices and audio playback |
| cursors | Loads cursor images and includes standard cursors |
| display | Controls the display window or screen |
| draw | Draws simple shapes onto a surface |
| event | Manages events and the event queue |
| font | Creates and renders TrueType fonts |
| image | Saves and loads images |
| joystick | Manages joystick devices |
| key | Manages the keyboard |
| mouse | Manages the mouse |
| movie | Used for the playback of MPEG movies |
| sndarray | Manipulates sounds with Numeric |
| surfarray | Manipulates images with Numeric |
| time | Controls timing |
| transform | Scales, rotates, and flips images |

> For more information on the PyGame library, you can visit www.pygame.org.

## Installing PyGame

PyGame usually comes installed with the latest Raspbian distribution, but if it isn't you can use the following command to install it:

```
sudo apt-get install python-pygame
```

Test your installation by opening a Python terminal by entering python in a regular terminal and pressing *Enter*. Now, execute the following command:

```
import pygame
```

Now that you have your system set up and you have hopefully checked out the PyGame website to explore its complete functionalities, we will move on to build the binary fractal tree to introduce you to the workings of PyGame. Let's begin!

## Drawing a binary fractal tree

A binary fractal tree is defined recursively by binary branching. Typically, it consists of a trunk of length 1, which splits into two branches of decreasing or equal length, each of which makes an angle Q with the direction of the trunk. Furthermore, both of these branches are divided into two branches, each making an angle Q with the direction of its parent branch, and so on. Continuing in this way, we can infinitely make branches, and the collective diagram is called a **fractal tree**. The following diagram visually shows what such a fractal tree might look like:

![](img/631e8b200cce187082ee46b0eb1681b3_65_0.png)

Now, let's move on to the code and take a look at how such a fractal tree can be constructed with PyGame. Following this paragraph is the complete code, and we will go through it statement by statement in further paragraphs:

```
import pygame
import math
import random
import time

width = 800
height = 600

pygame.init()
window = pygame.display.set_mode((width, height))
pygame.display.set_caption("Fractal Tree")
screen = pygame.display.get_surface()

def Fractal_Tree(x1, y1, theta, depth):
    if depth:
        rand_length=random.randint(1,10)
        rand_angle=random.randint(10,20)
        x2 = x1 + int(math.cos(math.radians(theta)) * depth * rand_length)
        y2 = y1 + int(math.sin(math.radians(theta)) * depth * rand_length)
        if ( depth < 5 ):
            clr = ( 0 , 255 , 0 )
        else:
            clr = ( 255, 255 , 255 )
        pygame.draw.line(screen, clr , (x1, y1), (x2, y2), 2)
        Fractal_Tree(x2, y2, theta - rand_angle, depth - 1)
        Fractal_Tree(x2, y2, theta + rand_angle, depth - 1)

Fractal_Tree( (width/2), (height-10) , -90, 12)
pygame.display.flip()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT or event.type == pygame.KEYDOWN:
            pygame.quit()
            exit(0)
```

Save the preceding code as prog1.py and then run the following:

```
python prog1.py
```

You will now get the following output:

![](img/631e8b200cce187082ee46b0eb1681b3_67_0.png)

If you increase the depth by 2 with a slight increase in the canvas area, the fractal tree now looks like this:

![](img/631e8b200cce187082ee46b0eb1681b3_67_1.png)

The new tree looks considerably denser and more branched out than the original tree.

Now, since you know what the output looks like, let's grab our magnifying glasses and sift through the program to understand how it works!

The first four lines are there to satisfy the dependencies required to build the program. These are PyGame, a math library, a library to generate random numbers, and a library to keep track of time for delay functions. Next, we specify the dimensions for the screen space required for our program.

The pygame.init() method initializes all the modules that were loaded as part of importing pygame in the first statement. It is required to be executed if you want to be able to use any functionality of PyGame. The display.set_mode() method creates a new Surface object, which represents the area on the screen that is visible to the user. It takes a tuple consisting of the dimensions of the window as the argument: the width and height variables in this case. It is literally the canvas on which you can draw. Anything you do to this object will be shown to the user. Images and other objects are represented as PyGame objects, and you can overlay them on the main surface. Then, we set the title of the window using the caption() method, and finally, the screen variable actually gets the object that the display is stored in. So now, our canvas is stored in the screen variable, and we will use it to make any changes to the canvas.

The Fractal_Tree function should be fairly easy to understand. It takes four arguments: the starting point of the branch (x1, y1), the angle of the branch with respect to the positive x axis — which is a horizontal line going to your right when you look at the computer screen, and the depth of the fractal tree (which indicates the levels in the tree). It is called for the first time in the 28th line with appropriate arguments. You might notice that towards the end of the function, it calls itself using different arguments. These kinds of functions are called recursive functions and are very useful in tasks where there is repetition and where the same task needs to be performed with minor differences.

If the depth is positive, it selects a random length and angle for the next branch by selecting a random integer from the randint() method of the random package. It then specifies the coordinates of the end of that branch, which are labeled x2 and y2. If the depth is less than 5, it selects the color of the line as green; otherwise, it is white. Here, it is important to understand that the color is represented in the RGB format. Hence, (0, 255, 0) means green. Similarly, (255, 0, 0) will be red. The three numbers in the tuple represent the intensity of RGB colors; hence, we can select any color using a mixture of these three intensities.

Finally, there is a recursive call to itself (`Fractal_Tree`), and the program then draws the second level of the fractal tree and so on until the depth becomes zero. There are two recursive calls: one to draw the left branch and the other to draw the right branch. If you've noticed, the function isn't actually executed until the 28th line. And once it is executed, the complete pattern is drawn at once due to the recursiveness of the function, but it still isn't displayed. The next line, `pygame.display.flip()`, is responsible for displaying the drawn shapes on screen:

```
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT or event.type == pygame.KEYDOWN:
            pygame.quit()
            exit(0)
```

This block of code is there to ensure that PyGame quits properly and specifies how to shut down the program. The `event.get()` method clears the event queue so that you always get the last event that occurred. An event queue consists of all the key presses and mouse clicks that happen, and they are stored in a **Last In First Out (LIFO)** fashion. You will see this `while` loop in almost every PyGame program as it handles the exit of the program properly. If you are using IDLE, then not shutting down PyGame properly can cause it to hang. In this case, PyGame quits when `pygame.quit()` is executed. Finally, with `exit(0)`, Python also quits and closes the application.

As we are randomizing the length and the angle every time the function calls itself, no two branches will be exactly the same, giving it the appearance of the irregularity of real-life trees. By induction, no two trees will be the same.

You can modify parts of this code to see for yourself how the tree behavior changes on changing a few variables, such as `theta` and `depth`. Now that we have completed all the basics of PyGame and can implement fairly complex problems, we are now ready to move on to a real challenge: making an actual game.

## Building a snake game

Who doesn't remember the classic game called Snake, which involves a snake chasing a morsel of food? It is probably the very first game that you played as a child. The basic premise of the game is that you control a snake and lead it to a morsel of food. Every time the snake consumes that food, it grows by one unit length, and if the snake hits a boundary wall or itself, it dies. Now as you can imagine, the more you play the game, the longer the snake grows, which, consequently, makes it more difficult to control the snake. In some versions of the game, the speed of the snake also increases, making it even more difficult to control. There comes a point where you simply run out of screen space and the snake inevitably hits a wall or itself, and the game is over.
