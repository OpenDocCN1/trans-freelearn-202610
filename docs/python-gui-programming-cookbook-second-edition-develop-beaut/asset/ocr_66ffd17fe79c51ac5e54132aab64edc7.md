Using the keyboard arrow keys, we can spin the 3D cube around:

![](bbox=[0.05, 0.14, 0.91, 0.5])

# How it works...

In this recipe, we used pyglet to create a colorful cube, which we can rotate in 3D space using the keyboard arrow keys.

We defined several colors for the six faces of our cube, and we used pyglet to create our main window frame.

The code is similar to a previous recipe in this chapter, in which we used the wxPython library to create a cube. The reason for this is that, underneath the hood, both wxPython and PyGLet use the OpenGL library.

# OpenGL animation

In the previous recipes, we used the wxPython and PyGLet frameworks to display OpenGL. In this recipe, we will use pure Python and OpenGL to create a red rectangle that bounces within a window that has a blue background.