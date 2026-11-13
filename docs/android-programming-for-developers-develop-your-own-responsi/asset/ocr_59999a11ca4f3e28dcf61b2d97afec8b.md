```
mTotalMillis = 0;
}
```

```
@Override
public void onUpdate(long elapsedMillis, GameEngine gameEngine) {
    mTotalMillis += elapsedMillis;
    if (mTotalMillis > 1000) {
        mFps = mDraws*1000 / mTotalMillis;
        mFpsText = mFps+ " fps";
        mTotalMillis = 0;
        mDraws = 0;
    }
}
```

```
@Override
public void onDraw(Canvas canvas) {
    mPaint.setColor(Color.BLACK);
    canvas.drawRect(0, (int)(canvas.getHeight()-mTextHeight),
                    mTextWidth, canvas.getHeight(), mPaint);
    mPaint.setColor(Color.WHITE);
    canvas.drawText(mFpsText, mTextWidth2, (int)
                    (canvas.getHeight()-mTextHeight/2), mPaint);
    mDraws++;
}
```

The logic is quite simple; we count the number of calls to onDraw and during onUpdate. Whenever we have been running for more than 1000 milliseconds, we do the calculation, store the result, and reset both variables.

The onDraw method draws a black square and then renders the text on it centered. We set the size of the square to 50 by 25 units and the text size to half the height of the square, so there is a margin. For this, we also need to use a Paint object.

The Paint class has methods to set colors, alignments, stroke width and type, and so on. Since we are mostly drawing bitmaps, we won't go into more detail on this.

Note that the Paint object is created once and reused, as we did with the Matrix on the Sprite, with (yet again) the premise that we should not do any object allocations inside onDraw.