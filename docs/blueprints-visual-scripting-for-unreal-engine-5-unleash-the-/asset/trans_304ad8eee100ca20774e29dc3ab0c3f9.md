

## 虚幻引擎 5 蓝图可视化脚本

第三版

释放蓝图的真正威力，在 UE5 中创建令人印象深刻的游戏和应用程序

Marcos Romero | Brenden Sewell

## Table of Contents

- Preface
- Part 1: Blueprint Fundamentals
- Chapter 1: Exploring the Blueprint Editor
- Chapter 2: Programming with Blueprints
- Chapter 3: Object-Oriented Programming and the Gameplay Framework
- Chapter 4: Understanding Blueprint Communication
- Part 2: Developing a Game
- Chapter 5: Object Interaction with Blueprints
- Chapter 6: Enhancing Player Abilities
- Chapter 7: Creating Screen UI Elements
- Chapter 8: Creating Constraints and Gameplay Objectives
- Part 3: Enhancing the Game
- Chapter 9: Building Smart Enemies with Artificial Intelligence
- Chapter 10: Upgrading the AI Enemies
- Chapter 11: Game States and Applying the Finishing Touches
- Chapter 12: Building and Publishing
- Part 4: Advanced Blueprints
- Chapter 13: Data Structures and Flow Control
- Chapter 14: Math and Trace Nodes
- Chapter 15: Blueprints Tips
- Chapter 16: Introduction to VR Development
- Part 5: Extra Tools
- Chapter 17: Animation Blueprints
- Chapter 18: Creating Blueprint Libraries and Components
- Chapter 19: Procedural Generation
- Chapter 20: Creating a Product Configurator Using the Variant Manager
- Other Books You May Enjoy.
- Appendix

## Preface

Unreal Engine's Blueprint visual scripting system enables designers to script their games as well as programmers to create base elements that can be extended by designers. With this book, you'll explore all the features of the Blueprint Editor, along with expert tips, shortcuts, and best practices.

The book guides you through using variables, macros, and functions, and helps you learn about object-oriented programming (OOP). You'll discover the Gameplay Framework and advance to learning how Blueprint Communication allows one Blueprint to access information from another Blueprint. Later chapters will focus on building a fully functional game step by step. You'll start with a basic first-person shooter (FPS) template, and each chapter will build on the prototype to create an increasingly complex and robust game experience. You'll then progress from creating basic shooting mechanics to more complex systems, such as user interface elements and intelligent enemy behavior. The book demonstrates how to use arrays, maps, enums, and vector operations and introduces the elements needed for VR game development. In the final chapters, you'll learn how to implement procedural generation and create a product configurator.

By the end of this book, you'll have learned how to build a fully functional game and have the skills required to develop an entertaining experience for your audience.

## Who this book is for

This book is for anyone interested in developing games or applications with Unreal Engine 5. Whether you are brand new to game development or have just not had any exposure to Unreal Engine 5's Blueprint Visual Scripting system, this is a great place to start learning about how to build complex game mechanics quickly and easily without writing any text code. No programming experience is required!

## What this book covers

- *Chapter 1, Exploring the Blueprint Editor,* covers the Blueprint Editor and all the panels that are integrated into it. We will explore the **Components** tab, the **My Blueprint** tab, the **Details** tab, and the **Viewport** and **Event Graph** tabs. Then, we will go through what components are and how to add them to a Blueprint.
- *Chapter 2, Programming with Blueprints,* explains programming concepts that are used in Blueprints. We will learn about how to use variables, operators, events, actions, macros, and functions.
- *Chapter 3, Object-Oriented Programming and the Gameplay Framework,* teaches OOP concepts and explores the Gameplay Framework.
- *Chapter 4, Understanding Blueprint Communication,* explores different types of Blueprint Communication, which allows one Blueprint to access the information of another Blueprint.
- *Chapter 5, Object Interaction with Blueprints,* covers how to bring new objects to a level to help build the world in which the game will be set. We will move on to manipulating materials on objects, first through the object editor, and then by triggering during runtime via Blueprints.
- *Chapter 6, Enhancing Player Abilities,* teaches you how to use Blueprints to generate new objects during gameplay, and how to link actions in Blueprints to player control inputs. You'll also learn about how to create Blueprints that allow objects to react to collisions with our generated projectiles.
- *Chapter 7, Creating Screen UI Elements,* demonstrates setting up a **graphical user interface (GUI)** that will track the player's health, stamina, ammo, and current objective. Here, you will learn how to set up a basic user interface using Unreal's GUI Editor and how to use Blueprints to link the interface to the gameplay values.
- Chapter 8, Creating Constraints and Gameplay Objectives, covers how to constrain the player's abilities, define the gameplay objectives for a level, and track those objectives. We'll walk through setting up collectible ammo packs that will refill the ammo of the player's gun, as well as utilizing the level Blueprint to define a win condition for our game.
- Chapter 9, Building Smart Enemies with Artificial Intelligence, is a crucial chapter that covers how to create an enemy zombie AI that will pursue the player around the level. We'll walk through setting up a navigation mesh on our level and see how to use Blueprints to get enemies to traverse between patrol points.
- Chapter 10, Upgrading the AI Enemies, teaches us how to create a compelling experience by modifying the zombie AI to have states in order to give the zombies a little more intelligence. In this chapter, we'll set up the patrol, searching, and attack states for the zombies by using visual and auditory detection. Additionally, we'll explore how to make new enemies appear gradually as the game is playing.
- Chapter 11, Game States and Applying the Finishing Touches, adds the finishing touches that are required to make our game a complete experience before we finalize our game for release. In this chapter, we'll create rounds that will make the game increasingly difficult, game saves so that the player can save their progress and return, and player death to make the game's challenge meaningful.
- Chapter 12, Building and Publishing, covers how to optimize graphics settings to get our game performing and looking its best, and how to set up project information for distribution. Then, we'll learn about how to create shareable builds of the game for various platforms.
- Chapter 13, Data Structures and Flow Control, explains what data structures are and how they can be used to organize data in Blueprints. We'll learn about the concept of containers and how to use arrays, sets, and maps to group multiple elements. This chapter shows other ways to organize data using enumerations, structures, and data tables. In this chapter, we'll also see how to control the flow of execution of a Blueprint by using various types of flow control nodes.
- Chapter 14, Math and Trace Nodes, covers some math concepts that are required for 3D games. We will learn the difference between world and local coordinates and how to use them when working with components. This chapter shows us how to use vectors to represent the position, direction, velocity, and distance. The concept of traces is explained and various types of traces are presented. We'll also see how to use traces to test collisions in the game.
- Chapter 15, Blueprints Tips, contains several tips to increase the quality of Blueprints. We will learn about how to use various editor shortcuts that speed up our work. This chapter demonstrates some Blueprint best practices that will help you decide where and what types of implementation should be undertaken. Finally, we'll learn about more useful Blueprint miscellaneous nodes.
- Chapter 16, Introduction to VR Development, explains some VR concepts and explores the VR template. This chapter explores the functionalities of the VRPwn Blueprint of the VR template and explains how to create objects that can be grabbed by the player using motion controllers. We will learn about the Blueprint functions used to implement teleportation and how to use the interface for Blueprint communication. We will also see how the menu works in the VR template.
- Chapter 17, Animation Blueprints, presents the main elements of the Unreal Engine animation system, including Skeleton, Skeletal Mesh, Animation Sequences, and Blend Spaces. It shows how to script an Animation Blueprint using the Event Graph and the Anim Graph. It explains how state machines are used in an animation and how to create new states for an animation.
- Chapter 18, Creating Blueprint Libraries and Components, shows how to create Blueprint Macro and Function Libraries with common functionalities that can be used throughout the project. It explains in more detail the concept of components. We will also learn how to create Actor components with encapsulated behavior and Scene components with location-based behavior.

