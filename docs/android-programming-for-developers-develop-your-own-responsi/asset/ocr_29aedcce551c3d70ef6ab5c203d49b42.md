The **Game Engine** is the component through which the rest of the program interacts with the game. Its mission is also to encapsulate the existence of the update and draw threads as well as to handle the game objects.

A game is composed of **Game Objects** that are both updated and drawn. These objects are held inside the **Game Engine**.

The **Update Thread** is responsible for updating the state of the game objects as fast as it can. It will run through all the game objects calling an update method.

The UI has to also be constantly updating and be independent of the update thread. It will draw all the game objects by calling a draw method on them.

Let's analyze each component in detail.