```
|| mPositionY > canvas.getHeight()
|| mPositionX < -mImageWidth
|| mPositionY < -mImageHeight) {
    return;
}
mMatrix.reset();
mMatrix.postScale((float) mPixelFactor, (float) mPixelFactor);
mMatrix.postRotate((float) mRotation,
            (float) mImageWidth/2, (float) mImageHeight/2);
mMatrix.postTranslate((float) mPositionX, (float) mPositionY);
canvas.drawBitmap(mBitmap, mMatrix, null);
}
```

We just have to check if the positions on the x and y axes and see if the sprite is inside the drawing area. Remember that we use the points at the top-left corner of the image. This is why we have to check against the width and height in negative.

This will not make any difference in our game, but it may be crucial for other types of games, especially those with static content.