# # Custom dialogs

The next step in making our UI nicer is to stop using the standard Android dialogs and create our own custom ones instead. Once again this is something completely discouraged for apps but it makes a lot of sense for games.

There are many reasons why a custom dialog system is better for games:

+   - The default dialogs will look different in different Android versions. This is in fact great when you use `AlertDialog` in an app, since it mimics the dialogs of all the rest of the apps, but not the games. We want consistency.
+   - Default dialogs use the system font.
+   - If you try to set a custom background or a custom content view, `AlertDialogs` look terrible. Again, you should never do that in an app, but you definitely want it in a game.
+   - The standard dialogs ruin the immersive experience by showing the notifications and system bars when they are on screen.
+   - It is hard to replace the animations of `AlertDialog` with custom ones.

We are going to build a system that shows our dialogs on screen, with a gray semi-transparent overlay behind them. These dialogs will have a custom background made out of a shape drawable and will use the custom font we have selected for the game.

Finally, the dialog itself is just a layout we put in front of the content.

It is worth noting that we need to define a policy for what to do when we are showing a dialog and a new one is trying to display on top. In our game, the old dialog will stay and the new one will not be displayed, but nevertheless we will provide a method to dismiss the previous one and show the new one in case you want that behavior in your game.

We are going to describe the architecture first (including modifications to `YassBaseFragment`), and then we will start by making the dialog for quitting the game because it is the simplest one. After that we will replace the current pause dialog and finally we will make a new dialog for game over.