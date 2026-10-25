Press Ctrl + Shift + A (Command + Shift + A) to quickly open the **Add Component** menu.
From there, you can type in the name of the Component you wish to add.

# # Scene window

Pressing _Shift + F_ or double-tapping the _F_ key will follow an object in the **Scene**
window (assuming that the **Scene** window is open and visible), which can be helpful for
tracking high-velocity objects or figuring out why objects might be falling out of our Scene.

Holding _Alt_ and left-click dragging with the mouse in the **Scene** window will make the
**Scene** window Camera orbit the currently selected object (as opposed to looking around it).
Holding _Alt_ and right-click dragging with the mouse in the **Scene** window will zoom the
Camera in/out (_Alt + Ctrl_ + left-drag).

Holding _Ctrl_ and left-click dragging will cause the selected object to snap to the grid as it
moves. The same can be done for rotation by holding _Ctrl_ as we adjust the rotation widgets
around the object. Selecting **Edit | Snap Settings...** opens a window where we can edit the
grid that objects snap to on a per-axis basis.

We can force objects to snap to each other through their vertices by holding the _V_ key as we
move an object around in the **Scene** window. By doing so, the selected object will
automatically snap its vertices to the nearest vertex, of the nearest object to the mouse
cursor. This is very useful for aligning Scene pieces into place, such as floors, walls,
platforms, and other tile-based systems, without needing to make small manual position
adjustments.

# # Arrays

We can duplicate array elements that have been exposed in the **Inspector** window by
selecting them and pressing _Ctrl + D_ (_Command + D_). This will copy the element and insert it
into the array immediately after the current selection.

We can remove entries from an array of references (for example, an array of `GameObjects`)
by pressing _Shift + Delete_ (_Command + Delete_). This will strip away the element and condense
the array. Note that the first press will clear the reference, setting it to `null`, but the second
press will remove the element. Removing elements in arrays of primitive types (_int_, `float`,
and so forth) can be accomplished by simply pressing _Delete_ without the _Shift_ key
(_Command_) modifier held down.