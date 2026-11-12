# Render

The `render` function has a Canvas object parameter, which is a blank canvas that we draw onto. The `canvas` class has many functions for drawing shapes and images directly onto the canvas, as follows:

```
canvas.drawRect(squarePos, squarePaint);
```

Here, we draw a rectangle on the canvas using the position of our sprite and using the color of the paint object to apply the paint styling. The paint object represents styling similar to CSS for the web by applying the styling to the square. In this example, the position is being updated manually by a fixed amount; however, in [Chapter 5, Moving the Graphics with Input], we will go into greater detail about how to control the position based on the user's input.

In Flame, we implement the game loop by extending one of the base classes, which automatically calls our `update` and `render` functions continuously. In the first example from [Chapter 1, Getting Started with Flutter Games], we used the `Game` mixin. This allowed us to override the `update` and `render` functions. This allowed us a lot of control over the process, but it can be very cumbersome once you start drawing and updating a lot of sprites. Then, when you start adding input controls and collision detection, it can quickly become difficult to maintain and keep track of everything.

Fortunately, Flame has classes to help with all this as your game grows, which we will discuss further in the next section.

# Working with components

As with any growing code base, it's important to have a structure. This is so that the code is easy to maintain as we add more features to the game.

Currently, our code contains a simple example of how to render and update a square on the screen, using the `Game` mixin to override these functions.