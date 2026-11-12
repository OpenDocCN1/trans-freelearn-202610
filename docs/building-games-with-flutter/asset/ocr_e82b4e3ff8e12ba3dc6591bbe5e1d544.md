# Chapter 9: Implementing Advanced Graphics Effects

So far in the book, we have used graphics for animating sprite components and drawing tile maps, but there is a lot more that we can do to improve the visual aspect of the game.

In this chapter, we are going to discuss how to use particles and shadows to improve our game. We will use particle effects to make the coins and enemies explode when they are collided with and we will use layers to add a shadow effect to our sprites. These are simple yet efficient ways to improve our game visuals that run very quickly and don't affect the frame rate too much, so they are worth using to improve our game.

We will cover the following topics in this chapter:

+   - What are particle effects?
+   - Animating with particles
+   - Creating shadows with layers

# Technical requirements

To examine the source from this chapter, you can download it from https://github.com/PacktPublishing/Building-Games-with-Flutter/tree/main/chapter09.

# What are particle effects?

Particle effects are an easy way to create dynamic effects such as fire, smoke, explosions, and magical effects for our games. Particles have various properties that can be changed, which include the following: