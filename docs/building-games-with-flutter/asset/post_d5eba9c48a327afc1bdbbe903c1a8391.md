
detecting whether we collide with the water and preventing George or the enemies from moving over it.

## Detecting tile collisions

So far in our game, we have used component-level collision detection to detect collisions between George and the coins and enemies. When you are working with map levels, you will generally add other objects on the map that act as barriers, such as water, buildings, or trees. As the player navigates around maps with these types of items, we want to ensure that neither George nor enemies walk through these objects.

In the *Understanding map navigation* section, we removed the `ScreenCollidable` component to allow the enemies to freely move around the map, but now the enemies and George can wander off the map.

In this section, we are going to add a water barrier that will go around the map. We will check for collisions to prevent both George and the enemies from being able to leave the map. We will read the map locations of the water from an object layer and create new `Water` components. We will make these components collidable and add these to the game so that Flame will check for collisions with the water and we can prevent George or the enemies from moving over the water.

This presents some interesting challenges because with our previous collision checks, we just remove the enemy or coin when George collides with them, and George can continue to move in the direction he is traveling in, but now we will have to prevent movement when we collide with the water. Before we get into that, let's first talk a little bit more about what is happening when Flame checks for collisions.

## Understanding collisions

When we set up a component to be collidable, we are adding a shape around the object. Then, Flame can use these shapes to calculate if one shape intersects with another shape and trigger a collision detection if that happens.

As the game grows, we keep adding more items that can be collided with, and the problem with this process is that for every collidable item we add to the game, there needs to be a collision check. The more collision checks we have, the more effort is required by the processor to handle all the math behind that. Eventually, if you have too many collision checks, the performance of the game will degrade, and you will notice that the game starts to slow down.

Let's look at some of the math that we have in the game so far, to put this in context:

- We have 12 enemies in the game, 6 zombies and 6 skeletons.
- We have 50 coins in the game.
- We are going to add borders around the edges of the map, so that could be around 200 water components due to each edge having 50 tiles with 4 edges around the map.
- We also have our player, George.

This is 263 collidable objects in the game. At the moment, every one of these collidable objects has a collision check with every other collidable object in the game. So, that is 263 * 263 = 69,169 collision checks. That's nearly 70,000 collision checks, and that's happening every frame!

That's a scary number of collision checks and it's surprising our game is still running!

If you think about it, most of those checks are a complete waste, because a coin cannot collide with other coins as they are all spread out around the map, and they don't move. This is the same for the water objects that we will add too. Also, the water objects can't collide with the coins either as they are in different locations.

Fortunately, Flame provides a great solution to this issue by allowing us to set `collidableType` on the collidable component to let us tell Flame whether we should do a check or not for this object.

The three collidable types are as follows:

- Active: An active collidable collides with other collidable objects that are of the active or passive type. This is the default collidable type, if you don't set the value for all collidable objects.
- Passive: A passive collidable collides with other collidables of the active type but not with other passive collidable objects.
- Inactive: An inactive collidable will not collide with other collidable objects.

The solution for us to reduce the number of collision checks is to make the `Water` and `Coin` objects passive and leave George and the enemy objects in their default active state.

### Implementing collisions

Let's get started by adding the `Water` objects to our game:

1. In the project's `component` folder, create a new file called `water.dart` and add the following code:

```dart
import 'package:flame/components.dart';
import 'package:flame/geometry.dart';
class Water extends PositionComponent with
    HasHitboxes, Collidable {
  Water({required Vector2 position, required Vector2 size, required this.id}) : super(position: position, size: size);

  int id;
  @override
  Future<void> onLoad() async {
    super.onLoad();

    collidableType = CollidableType.passive;

    addHitbox(HitboxRectangle());
  }
}
```

Here, we define the `Water` object, which is a rectangular area that has a position and size.

In the `onLoad` function, we set `collidableType` to be of the `CollidableType.passive` type and add a hitbox for the collision checks between `George` and `Water` and the enemies and `Water`.

2. Open the `main.dart` file and at the bottom of the `onLoad` function, add the following code below where we added the coins:

```dart
final water =
    tiledMap.tileMap.getObjectGroupFromLayer('Water');
water.objects.forEach((rect) {
  add(Water(position: Vector2(rect.x, rect.y), size:
      Vector2(rect.width, rect.height), id: rect.id));
});
```

Here, we get the `water` objects from the tile map as an object group and iterate over these objects, using the x, y, width, and height values from this to create and add a `Water` component to the game.

If you open the tile map in Tiled, you can see that the `Water` object layer consists of four simple rectangles, which reduces the amount of collisions checks. We don't really care about which individual water tile we collide with, so we can just check the four edges.

3. At the top of the same file, add the following import for the `Water` component:

```dart
import 'package:goldrush/components/water.dart';
```

4. Open the `coin.dart` file and add the following code to the `onLoad` function to set `collidableType` to be passive:

```dart
super.onLoad();
collidableType = CollidableType.passive;
```

5. Let's change the collision for the enemies first as it's a very quick change. Open the `character_enemy.dart` file and at the top of the onCollision function, let's change the object check to use the Water object instead of ScreenCollidable with the following code:

```dart
if (other is Water) {
```

6. Next, we need to add the import for the Water class at the top of this file:

```dart
import 'package:goldrush/components/water.dart';
```

Moving on, Flame has debugMode, which we can set to true to see the bounding boxes (a box that shows the boundary limits of the area of detection) of the collidable boxes, which is useful for debugging collision detection. Let's add that next.

7. Open the main.dart file and at the top of the onLoad function, add the following line:

```dart
debugMode = true;
```

If you run the game now, you will see the bounding boxes and you may notice an issue.

The Skeleton and Zombie sprites have a large amount of space at the top of the sprites, which means if George collides with the top of these sprites, the collision will happen sooner than expected because of the extra space in the image. To get around this, we can pass two values to HitboxRectangle, which are relation and relativeOffset.

The relation value defines the relationship between the length of the horizontal and vertical sides and the size of the bounding box, and the relativeOffset value is the position of your shape in relation to its size from (-1,-1) to (1,1).

To fix the space issue at the top of the enemy sprite classes, open both the zombie.dart and skeleton.dart files, and at the bottom of the onLoad function, change the line that adds the hitbox in both classes to the following code:
