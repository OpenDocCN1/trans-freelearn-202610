Next, we bind our framebuffer object as buffer to read from, and we select the color attachment 1 of the framebuffer, containing the selection texture:

```
glBindFramebuffer(GL_READ_FRAMEBUFFER, mBuffer);
glReadBuffer(GL_COLOR_ATTACHMENT1);
```

Then, we adjust the internal alignment used during the read process, and read out the color value of a single pixel at the given xPos and yPos position:

```
glPixelStorei(GL_UNPACK_ALIGNMENT, 1);
glReadPixels(xPos, yPos, 1, 1, GL_RED, GL_FLOAT, &pixelColor);
```

Finally, we switch the framebuffer back to color attachment 0, unbind the buffer and return the pixel color:

```
glReadBuffer(GL_COLOR_ATTACHMENT0);
glBindFramebuffer(GL_READ_FRAMEBUFFER, 0);
return pixelColor;
}
```

# Adding mouse button handling

Selecting an instance will be added to the left mouse button. It feels natural to click on an instance to select it. To store the status of the selection, a Boolean member variable named mMousePick is added to the renderer.

Then, the following line must be added to the handleMouseButtonEvents() method of the renderer:

```
if (button == GLFW_MOUSE_BUTTON_LEFT &&
    action == GLFW_RELEASE) {
    mMousePick = true;
}
```

Using the GLFW_RELEASE action here, reacting when the left mouse button is released, matches the selection style of many applications. If you want to change the behavior, you can use the action GLFW_PRESS.