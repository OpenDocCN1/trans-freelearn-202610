Now we can see how things look with the label returned to the scene:

![](bbox=[0.28, 0.12, 0.71, 0.29])

Figure 2.10 – Main Scene with the GreetingsLabel node returned

We can now drag and drop several instances of the greetings_label scene into the Main scene, and we can change the text property of each one in the Text property of the Inspector. However, when we run the Main scene, each label reverts to Go Go Godot!. This is because each instance has the same script attached to it, and we previously changed the text property to read Go Go Godot!.

![](bbox=[0.28, 0.43, 0.71, 0.61])

Figure 2.11 – Multiple instances of greetings_label in the Main scene

# What is an instance?

Instancing is the process of producing a game object from a blueprint or primary design. Instancing works well with scenes. It gives you the ability to divide your game into reusable components, a tool to structure complex systems, and a language to think about your game structure in a natural way.

We can demonstrate instancing by allowing our game to react to input from the user.