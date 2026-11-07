This is where delta comes in.

delta is a small number that tells us how much time has passed since the last frame was drawn. If your game is running fast, delta might be something such as 0.016 (about 1/60th of a second). If it’s running slower, delta will be a bit bigger, such as 0.03.

By multiplying the movement speed by delta, we’re saying the following:

> “Move this object based on how much time has actually passed, not how many frames have gone by.”

This means the object will move at the correct speed per second, no matter how many frames your computer is managing to draw.

So, instead of moving 150 pixels every frame (which would be way too fast!), we move 150 × delta pixels per frame, which adjusts automatically based on performance.

# Continuing the bouncing label project

Bringing this back to our bouncing label project, the next step is to set up some important information about the screen and the label itself. This will let us detect when the label hits the edges and needs to bounce.

Now, we should add variables to store the screen size and the size of the label. It’s important to understand that we cannot determine these values until the scene has loaded.

The first function to run once the scene is ready is the _ready() function. So, we should initialize (assign values to) our variables there. First, add the variables to our collection at the top and then assign them as the first lines in the _ready() function after setting the text:

```
var xVelocity = 150 #150 pixels per second to the right
var yVelocity = 120 #120 pixels per second downwards
var velocity = Vector2(xVelocity, yVelocity) # Initial speed and direction
var screen_size_x #screen horizontal dimension
var screen_size_y #screen vertical dimension
var label_size_x #label width
var label_size_y #label height
```