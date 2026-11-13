# # General concepts

We are going to make our particle system in a way that mimics the Leonids particle system library at http://plattysoft.github.io/Leonids/. This project is a free software library I made to use particle systems in the standard Android UI.

![](bbox=[0.113, 0.231, 0.708, 0.448])

A demo of the Leonids library is available on Google Play

As a result of this, Leonids implements its own update and draw threads, as well as a simplified GameView. We already have all these, so we will adapt the code to suit our needs.

The concepts we are going to work with in this chapter are:

+   - **Particle**: Each of the sprites that are rendered
+   - **Particle system**: The entity that takes care of spawning, updating, and keeping track of the particles
+   - **Initializer**: A class that sets a value for a particle based on certain parameters (before being active)
+   - **Modifier**: A class that updates a value for a particle based on the elapsed time (while being active)

In our game, we will use only a few initializers and modifiers. If you need to extend the functionality of the particle system, you can visit Leonids' GitHub page to get ideas from it.