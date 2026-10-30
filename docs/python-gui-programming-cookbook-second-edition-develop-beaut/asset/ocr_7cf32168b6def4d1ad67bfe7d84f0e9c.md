We have slightly improved the sample code by making the image turn when holding down one of the keys instead of having to press and release the key.

# Getting ready

The previous recipe, *PyGlet transforms our GUI easier than PyOpenGL*, explains how to install PyGlet and gives you an introduction to this library. If you have not done so, it is probably a good idea to browse through that recipe.

> In the online documentation, PyGlet is usually spelled in all lower-case. While this might be a Pythonic way, we capitalize the first letter of a class and we use lower case for variable, method, and function names to start each name.

# How to do it...

The following code creates the 3D colored cube, which follows the code. This time, we will use the keyboard arrow keys to rotate the image instead of the mouse:

```
```python
import pyglet
from pyglet.gl import *
from pyglet.window import key
from OpenGL.GLUT import *

WINDOW    = 400
INCREMENT = 5

class Window(pyglet.window.Window):

    # Cube 3D start rotation
    xRotation = yRotation = 30

    def __init__(self, width, height, title=''):
        super(Window, self).__init__(width, height, title)
        glClearColor(0, 0, 0, 1)
        glEnable(GL_DEPTH_TEST)

    def on_draw(self):
        # Clear the current GL Window
        self.clear()

        # Push Matrix onto stack
        glPushMatrix()
```
```