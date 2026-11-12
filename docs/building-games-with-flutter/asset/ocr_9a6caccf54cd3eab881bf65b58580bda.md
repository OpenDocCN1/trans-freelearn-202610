```
spriteImages, srcSize: Vector2(georgeSizeWidth,
georgeSizeHeight));
sprite = spriteSheet.getSprite(0, 0);
position = Vector2(centerX, centerY);
size = Vector2(georgeSizeWidth, georgeSizeHeight);
}
```

Here, we use the Flame helper class to load the sprite sheet images and set up the sprite.

For now, we are going to get the first sprite frame and show only that frame. The first frame we get is 0, 0 in the sprite sheet:

![](bbox=[0.345, 0.345, 0.65, 0.545])

Figure 4.1 – Sprite at 0, 0

+   6. Now, let's switch to the `main.dart` file, so we can add the George component to see it on the screen. In the `onLoad` function, we can remove the lines that add the `Player` and `ScreenCollidable` components, and add the `George` component, like this:

```
@override
Future<void> onLoad() async {
super.onLoad();
add(George());
}
```

+   7. We can now remove the following imports of the `Player` and `ScreenCollidable` components that were used to show the green square, which we used in Chapter 2, Working with the Flame Engine: