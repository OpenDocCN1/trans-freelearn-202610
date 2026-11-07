# Technical requirements

To start with the chapter, you should know how to do the following:

+   - Create a new Godot project
+   - Create nodes
+   - Create scenes

You should also know about variables and functions for use in GDScript (see Chapter 4).

This chapter’s code files are available here in the book’s GitHub repository: https://github.com/PacktPublishing/Godot-4-for-Beginners/tree/main/ch6

Visit this link to check out the video of the code being run: https://packt.link/DcrOS

The game assets used in this project are released under a Creative Commons Zero (CC0) license by Pixel Frog. Pixel Frog has allowed us to distribute, remix, adapt, and build upon the material in any medium or format, even for commercial purposes.

They can be found here:

+   - **Pixel Adventure:** https://pixelfrog-assets.itch.io/pixel-adventure-1
+   - **Pixel Adventure 2:** https://pixelfrog-assets.itch.io/pixel-adventure-2

# Building the level with a TileMap

Building 2D levels with TileMaps is an efficient and organized way to create game environments by reusing small, pre-designed tiles. This approach not only saves time and resources but also allows for easy adjustments and consistent visuals across your level design.

In this section, we’ll build the foundation of our 2D platformer by creating a level with a TileMap, allowing us to efficiently design and organize the game environment. TileMaps are essential because they make it easy to create detailed, grid-based levels while optimizing performance, keeping game worlds manageable and visually consistent.

Let’s begin the process of building our level by creating a new scene and adding essential nodes. Follow these steps to set up a functional, well-organized level using a TileMapLayer:

+   1. Create a new Godot Project and give it a name – I have called it PixelAdventure.
+   2. We need a scene to represent the first level of the game. To do this, we create a new 2D scene, which will have Node2D as the root. Rename Node2D to Level1.