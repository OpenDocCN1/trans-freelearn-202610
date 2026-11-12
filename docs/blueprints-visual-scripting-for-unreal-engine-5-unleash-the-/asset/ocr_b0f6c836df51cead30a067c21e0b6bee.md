The following diagram shows the hierarchy of **Common Classes**. In Unreal Engine, there is a parent class called **Object**. Classes inherit the characteristics of the class above it, which is their parent class. Based on the inheritance concept of OOP, we can state that an instance of the **Character** class is of type **Character**, type **Pawn**, and type **Actor**:

![](bbox=[0.14, 0.22, 0.76, 0.5])

Figure 3.24 – Hierarchy of Common Classes

By analyzing this hierarchy, we can see that the **Actor Component** and **Scene Component** classes are not Actors. These classes are used to create Components that can be added to Actors. Two examples of Components are `Static Mesh Component` and `Rotating Movement Component`, which we used in previous examples. We will cover the creation of Components in _Chapter 18, Creating Blueprint Libraries and Components_.

Let's take a closer look at some of the **Common Classes**.

# # Pawn

**Pawn** is a child class of **Actor**. A **Pawn** is an **Actor** that can be possessed by a **Controller** in the game. The **Controller** class represents a player or an **artificial intelligence** (AI). Conceptually, an instance of the **Pawn** class is the physical body of the game character, whereas an instance of the