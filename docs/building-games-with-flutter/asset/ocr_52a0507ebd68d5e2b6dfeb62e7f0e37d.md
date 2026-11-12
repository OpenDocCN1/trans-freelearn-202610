11. Next, let's update where we create the `George` class by passing the `position`. Let's also change the `priority` of the `George` component to fix a loading issue that only happens on the web:

```
```
var george = George(hud: hud, position:
    Vector2(gameScreenBounds.left + 300,
        gameScreenBounds.top + 300), size: Vector2(48.0,
        48.0), speed: 40.0);
add (george);
children.changePriority(george, 15);
```
```

12. Where we create and load the `TiledComponent` data, we now need to wrap it with our new `TileMapComponent`:

```
```
final tiledMap = await
    TiledComponent.load('tiles.tmx', Vector2.all(32));
add(TileMapComponent(tiledMap));
```
```

13. Let's change the positions of the `Skeleton` and `Zombie` classes next:

```
```
if (index % 2 == 0) {
    add(Skeleton(position: Vector2(position.x +
        gameScreenBounds.left, position.y +
        gameScreenBounds.top), size: Vector2(32.0,
        64.0), speed: 60.0));
} else {
    add(Zombie(position: Vector2(position.x +
        gameScreenBounds.left, position.y +
        gameScreenBounds.top), size: Vector2(32.0,
        64.0), speed: 20.0));
}
```
```

14. Next, let's fix the `Coin` class with the new `position`:

```
```
double posCoinX = (randomX * 32) + 5 +
    gameScreenBounds.left;
double posCoinY = (randomY * 32) + 5 +
    gameScreenBounds.top;
```
```

15. And next, let's fix the `Water` class' `position`: