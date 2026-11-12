# The Enemy logic

Enemy needs to be able to find and run toward Hero. We will achieve this by searching for the Hero character, calculating the difference in direction, and facing Enemy toward Hero.

# Adding the Enemy AI

Let's go back to Unreal Engine and focus on Content Browser. We now need an opponent. To add this, follow these steps:

+   1. Right-click and select Blueprint.
+   2. At the lower end of the window, let's drop all the classes and search for AIController.
+   3. Select AIController under Controller and hit Select in the lower-right corner.
+   4. We will name this AIController Enemy.
+   5. Open Enemy AIController and go to the EventGraph section.

First, we must find Hero and then store it in a local variable to be used at any time. To do so, follow these steps:

+   1. Right-click in an empty area within EventGraph and search for Event Begin Play.
+   2. Pull from the exec pin and search for Get All Actors Of Class.
+   3. Set the Actor Class pin to Hero.