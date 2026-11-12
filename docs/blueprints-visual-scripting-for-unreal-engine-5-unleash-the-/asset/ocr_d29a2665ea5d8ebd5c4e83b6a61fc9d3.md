These elementary concepts of OOP will help us understand the Gameplay Framework. Unreal Engine has some essential classes that are used in the development of games. These classes are parts of the Gameplay Framework. The main class of the Gameplay Framework is **Actor**.

# # Managing Actors

The Actor class contains all the functionality an object needs to exist in a Level. Therefore, the Actor class is the parent class for all objects that can be placed or spawned in a Level. In other words, any object that can be placed or spawned in a Level is a subclass of the Actor class. Most of the Blueprints that we'll create will be based on the Actor class itself or its child classes. Therefore, the features we will look at in this section will be useful for these Blueprints.

# # Referencing Actors

Variable types such as integer, float, and Boolean are known as primitive types because they only store simple values of the specified type. When working with objects or Actors, we use a type of variable known as an **object reference**. References in Blueprints allow different objects to talk to each other. We will explore this communication in greater detail in _Chapter 4, Understanding Blueprint Communication_.

For example, the following diagram represents instances of two Blueprint Classes in memory. The instance of the `BP_Barrel` Blueprint Class has an integer variable named `Hit Counter`, with a current value of `2`. The other variable, named `BP_Fire`, is an object reference, which is referencing an instance of **Blueprint Effect Fire**. We can access the public variables and functions of another Blueprint using an object reference variable: