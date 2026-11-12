# Chapter 10: Making Intelligent Enemies with AI

The game is coming along nicely now, but there isn't really any challenge to it yet. We collect the coins or kill our enemies, but that's all very predictable and easy.

In this chapter, we are going to change the game to make it more challenging by adding a health value to our player, George, and making the enemies chase us instead, reducing our health for each enemy that hits us. If our health gets to zero, we lose the game. So, the objective will be to collect the coins while avoiding the enemies.

To make the enemies appear more intelligent, we will use very simple **Artificial Intelligence (AI)** algorithms for the enemies to detect when George is nearby and, when they can see him, their movement will change from random movements to moving in George's direction to attack him.

We will then add some extra water to the map as obstacles and discuss how to move from your origin to the destination while avoiding the water and walking around it to reach your destination.

We will cover the following topics in this chapter:

+   - Making enemies chase the player
+   - Navigating obstacles with pathfinding

# Technical requirements

To examine the source from this chapter, you can download it from https://github.com/PacktPublishing/Building-Games-with-Flutter/tree/main/chapter10.