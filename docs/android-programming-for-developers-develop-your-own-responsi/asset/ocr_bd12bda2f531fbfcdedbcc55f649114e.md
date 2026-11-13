```
}
}
```

We are drawing the image at the y coordinate. When this coordinate is greater than 0, there will be space on the top of the view that needs to be filled with another image. This is what the first part of the code does; it draws the image one more time, but translates all the height of the image, so they tile.

In any case, we have to draw the background at mPositionY, which is what the second block does. Note that, when the position is smaller than 0, we only need to draw one image. The logic of the class ensures that the value of γ is never smaller than mImageHeight – mScreenHeight.

Once the γ position gets out of the screen, the second part of the drawing is no longer needed, so we subtract the image's height. With this, the image keeps the same position and scrolls smoothly, because the second part of the draw is now equivalent to the first one, which is not entered.

![](bbox=[0.115, 0.445, 0.825, 0.74])

The different states of the background drawable.

While this code works, it is quite inefficient, because it does a lot of drawing that is not visible.