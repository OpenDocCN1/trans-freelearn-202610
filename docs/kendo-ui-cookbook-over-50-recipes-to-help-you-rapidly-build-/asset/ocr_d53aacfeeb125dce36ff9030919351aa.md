In the preceding screenshot, the first icon from the left is the custom icon. When you click on
the custom icon, an alert message that displays the title of the window is shown. For a given
window, you can add only one custom icon.

# # Using the Window API to act on a window object

In this recipe, we will look at several methods that can be used to act on the window object.

# ## How to do it...

There are various methods available that help you act on an existing window object.
These include the following methods:

+   - **center**: This will center the window.
+   - **open**: This will open the window.
+   - **close**: This will close the window.
+   - **refresh**: This will refresh the content of window. Optionally, some parameters
+   can be passed to this function, which will override the default options.
+   - **maximize**: This will maximize the window to the entire viewing area.
+   - **minimize**: This will minimize the window to its title bar.
+   - **restore**: This will restore the window from its minimized or maximized state.
+   - **pin**: This will pin the window. It will fix the window's position, and hence,
+   when you scroll, the window will appear at the same position.
+   - **unpin**: This will unpin the window and remove the fixed position.
+   - **toggleMaximization**: This toggles the window between the maximized and
+   restored state.

Let's test these functions by creating a window and adding buttons that will execute the
corresponding actions when clicking on them:

The following is HTML markup that contains buttons:

```
```
<button id="btnOpen">Open</button><br>
<button id="btnClose">Close</button><br>
<button id="btnRefresh">Refresh</button><br>
<button id="btnMaximize">Maximize</button><br>
<button id="btnToggleMaximization">Toggle Maximization</button><br>
<button id="btnMinimize">Minimize</button><br>
<button id="btnRestore">Restore</button><br>
<button id="btnPin">Pin</button><br>
<button id="btnUnpin">Unpin</button><br>
```
```