
```
Flame.images.load('zombie_n_skeleton.png');
final spriteSheet = SpriteSheet(image:
spriteImages, srcSize: size);
downAnimation = spriteSheet.createAnimation(
row: 0, stepTime: 0.2, from: 0, to: 2);
leftAnimation = spriteSheet.createAnimation(
row: 1, stepTime: 0.2, from: 0, to: 2);
upAnimation = spriteSheet.createAnimation(
row: 3, stepTime: 0.2, from: 0, to: 2);
rightAnimation = spriteSheet.createAnimation(
row: 2, stepTime: 0.2, from: 0, to: 2);
changeDirection();

addHitbox(HitboxRectangle());
}
}
```

Once again, the amount of code is small, just setting up the animation in the onLoad function. As it extends the Character class, we get all the same code we have for George too.

The main difference here is we use the built-in sprite sheet's createAnimation function instead of our extension function, as this sprite sheet is in rows and not columns. For the zombies, the animation frames are from columns 0 to 2, so we add those parameters in the createAnimation call too.

- 11. Now, let's set up the skeleton enemy. In the components folder, create a new file called skeleton.dart and add the following code:

```
import 'package:flame/components.dart';
import 'package:flame/flame.dart';
import 'package:flame/geometry.dart';
import 'package:flame/sprite.dart';
import 'package:goldrush/components/character.dart';
class Skeleton extends Character {
Skeleton({required Vector2 position, required
Vector2 size, required double speed}) : super(
position: position, size: size, speed: speed);
@override
Future<void> onLoad() async {
super.onLoad();
var spriteImages = await
Flame.images.load('zombie_n_skeleton.png');
final spriteSheet = SpriteSheet(
 image: spriteImages, srcSize: size);
 downAnimation = spriteSheet.createAnimation(
 row: 0, stepTime: 0.2, from: 3, to: 5);
 leftAnimation = spriteSheet.createAnimation(
 row: 1, stepTime: 0.2, from: 3, to: 5);
 upAnimation = spriteSheet.createAnimation(
 row: 3, stepTime: 0.2, from: 3, to: 5);
 rightAnimation = spriteSheet.createAnimation(
 row: 2, stepTime: 0.2, from: 3, to: 5);
 changeDirection();

 addHitbox(HitboxRectangle());
 }
}
```

As with the zombie code, it's compact and inherits most functionality from the `Character` component. Because of the way the sprite sheet image is set up, we set the `from` and `to` fields to 3 to 5, respectively, to grab the correct animation frames for the skeleton.

Now we have the zombies and skeletons set up, let's add some extra collision code to the `George` class so that every time the `George` sprite collides with either a zombie or skeleton, it will delete the enemy from the screen as if the enemy had been killed.

- 12. At the bottom of the `George` class, add the following `onCollision` function:

```
@Override
void onCollision(Set<Vector2> points,
Collidable other) {
super.onCollision(points, other);
if (other is Zombie || other is Skeleton) {
other.removeFromParent();
}
}
```

- 13. Next, add the imports at the top of the `George` file for the `Zombie` and `Skeleton` classes:

```
import 'package:goldrush/components/skeleton.dart';
import 'package:goldrush/components/zombie.dart';
```

- 14. Finally, let's add the same imports that we just added to the George file to the main.dart file. Then, in the onLoad function, let's add the following code to add some zombies and skeletons and update George to pass the position, size, and speed:

```
add(George(position: Vector2(200, 400),
size: Vector2(48.0, 48.0), speed: 40.0));
add(Zombie(position: Vector2(100, 200), size:
Vector2(32.0, 64.0), speed: 20.0));
add(Zombie(position: Vector2(300, 200), size:
Vector2(32.0, 64.0), speed: 20.0));
add(Skeleton(position: Vector2(100, 600), size:
Vector2(32.0, 64.0), speed: 60.0));
add(Skeleton(position: Vector2(300, 600), size:
Vector2(32.0, 64.0), speed: 60.0));
```

If you run the code now, you will see a few zombies and skeletons and George wandering around, randomly changing direction every few seconds and when they hit the edges. If George collides with an enemy, the enemy will be removed from the screen.

Here is a picture of how the game looks so far:

![](img/a63c3fccbe888cd4d04006b5e1b6c273_75_0.png)

Figure 4.4 – George and the enemy sprites

In this section, we added enemy sprites to the game and collision code to detect when George collides with the enemies. We extracted the common code for movement and animation changes to a base class called `Character` and then built the sprite classes using this.

## Summary

In this chapter, we showed you how to load graphics from a sprite sheet and extract and play the animations. Then, we showed you how to move the sprites around and change the animation to match the direction of travel. Finally, we showed you how to animate multiple sprites independently and detect when they collide with each other or the screen edges.

In the next chapter, we will add more interactivity by learning how to control your player character with virtual joysticks and buttons. We will also look at other methods of input control, including detecting touch events on the screen and using them to control the player character.

## Questions

- 1. What is a sprite?
- 2. What functions should we use for the code which initializes the animations?
- 3. What are the benefits of using `SpriteAnimationComponent`?
- 4. What parameters should you set in the `createAnimation` function to specify a range within a sprite sheet?
- 5. What is the benefit of creating a base class for all our sprites?

# Chapter 5: Moving the Graphics with Input

In this chapter, we will take control of our own character and move them around the screen with virtual joysticks and touch events, so the player can choose where they want to move their character.

We will start by looking at how to draw onscreen controls that behave like a thumb-stick on a PlayStation controller, with action buttons for things such as attacking and jumping. Next, we will connect the virtual controls with our George sprite and get him moving around the screen. And finally, we will discuss an alternative or addition to virtual controls, by controlling George with touch events, such as tapping on the screen and moving George to a tapped location.

So, we will cover the following topics:

- Drawing onscreen controls
- Moving our character with onscreen controls
- Moving our character with touch

## Technical requirements

To examine the source from this chapter, you can download it from https://github.com/PacktPublishing/Building-Games-with-Flutter/tree/main/chapter05.

You can find additional information on Flame input in the online documentation at https://docs.flame-engine.org/1.0.0/gesture-input.html and https://docs.flame-engine.org/1.0.0/other-inputs.html.

In the next section, we will start by adding the onscreen controls and drawing them on the screen.

## Drawing onscreen controls

In this section, we will add a joystick and a button to the screen that will allow us to control the character and some text for showing the player their score. These three components will form part of our **Heads-Up Display** (**HUD**), which is part of the user interface, showing game information that is drawn over the other graphics in the game and remains in a fixed position. The type of information could be the player's health, the number of lives remaining, or, in our case, the game score.

We will encapsulate our HUD into a single component, which makes showing or hiding it easier. The HUD component will contain `JoyStickComponent` for controlling the direction that George moves; the joystick will work by dragging an onscreen circle within a larger circle in the direction you want to move. The HUD will also include `HudButtonComponent`, an onscreen button that the player can press to double George's walking speed, making him run.

We will connect the HUD components in the next section, but for now, let's draw them on the screen:

- 1. First, create a folder inside the components folder called `hud` to hold our components.
- 2. Inside the `hud` folder, create four files called `hud.dart`, `joystick.dart`, `run_button.dart`, and `score_text.dart`.
- 3. In the `run_button.dart` file, add the following imports and class definition:

```
import 'package:flame/components.dart';
import 'package:flame/input.dart';
import 'package:flutter/material.dart';
class RunButton extends HudButtonComponent {
}
```
