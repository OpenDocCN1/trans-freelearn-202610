+   - Identify a window
+   - Interact with application controls
+   - Extract data from a window
+   - Fill in data in a window

Let’s take a closer look at each of these actions before we dive into another example to leverage these actions.

# Identify a window

The first thing we need to do is get the window of the application we want to control. We already know that there are several ways to start a program, such as by utilizing the *run application* action or using a dedicated action for a program such as Excel or Outlook. The following actions identify a window or make it accessible for PAD, especially in combination with other windows:

+   - **Get window/focus window**: This can be found in the **Windows** subgroup. You can get or focus a running window and return a title and/or Window handle.
+   - **Set window state/set window visibility**: This can be found in the **Windows** subgroup. You can set the state of a window to **restored**, **maximized**, or **minimized**, which hides a window or makes it visible again, respectively.
+   - **Move/resize/close window**: Also in the **Windows** subgroup, this action can move a window to a specific position of the desktop by providing *X* and *Y* coordinates, can change the size of a window to a specific width and height, or just close a window.
+   - **If window/wait for window/wait for window content**: This action executes actions in a conditional block, depending on whether a window is open/focused or not, waits until a window is opened/closed/becomes the focus/loses focus, and waits for a specific UI element that should be contained in a window.