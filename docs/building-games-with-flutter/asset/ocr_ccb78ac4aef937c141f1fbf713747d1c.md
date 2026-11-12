```
(i) => from + i)
            .map((e => getSprite(e, column))
            .toList();
        return SpriteAnimation.spriteList(
        spriteList,
        stepTime: stepTime,
        loop: loop,
        );
        }
        }
```

This extension will create a list of sprites based on the column and then create a `SpriteAnimation` from the sprite list.

+   4. Now, we can use the sprite list to create an animation that we will assign to our `SpriteAnimationComponent` animation field. At the end of the `onLoad` function, add the following code:

```
animation = spriteSheet.createAnimationByColumn(
 column: 0, stepTime: 0.2);
```

Here, we call the new `createAnimationByColumn` extension function, passing `0` for the first column animation, and the `stepTime` animation speed to `0.2`, which gives the impression of a normal walking speed in the animation.

Now, we have expanded our sprite by drawing multiple frames to create an animation.

In the next section, we will set up animations for each of George's directions and move him around the screen, changing directions every few seconds, and changing the animation to match the direction he is traveling in.

# Moving a sprite around the screen

Now that we have the George sprite animating, we will build on that and change the animation based on the direction of travel: