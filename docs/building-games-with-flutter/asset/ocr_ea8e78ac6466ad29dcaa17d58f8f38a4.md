# Chapter 7

+   1. The Tiled application allows us to create tile maps that are much larger than the physical screen of our game, by using tile sets made up of small tiles to represent things like grass or water.

+   2. Tile maps reuse each tile, meaning that they take up much less memory than storing a larger image.

+   3. Tile map data is stored in a 2D array to represent the width and height of the map.

+   4. We can use tile layers for representing the tiles and object layers for objects we want to draw on top of the map.

+   5. To adjust the map as a sprite moves around, we use a camera and set up the `followComponent` function with the component that we want to focus on while it moves.

+   6. We can add collidable objects as an object layer in our tile map and then create components from these by reading the object with the tile map `getObjectGroupFromLayer` function.

+   7. A collidable object can be active, passive, or inactive. We use these to reduce the amount of collision checks between collidable objects, which helps the game's performance.