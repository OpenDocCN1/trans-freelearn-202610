Having duplicate **Inspector** windows can be kind of redundant since they'll show the exact same information when we click on a new object. However, by making use of the _lock_ icon, we can lock the given **Inspector** window to its current selection. When we select an object, all **Inspector** windows will update to show the object's data, except for any locked **Inspector** windows, which continue to show the data of the object they were locked to.

![](bbox=[0.302, 0.219, 0.697, 0.283])

Common tricks that make use of window locking include the following:

+   - Using two of the same window (**Inspector**, **Animation**, and so forth) to compare two objects side by side or more easily copy data from one object to another.
+   - Watching what happens to any dependent object if an object is tweaked during **Play Mode**.
+   - Selecting multiple objects in the **Project** window, then dragging and dropping them into a serialized array in the **Inspector** window without losing the original selection.

# # The Inspector window

We can enter calculations into numeric **Inspector** window fields. For example, typing `4*128` into an `int` field will resolve the value to `512`, sparing us from having to pull out a calculator or do the math in our head.

Array elements can be duplicated and deleted from a list (in the same fashion as the hotkeys mentioned previously) by right-clicking on the root element and selecting **Duplicate Array Element** or **Delete Array Element**.

A Component's context menu can be accessed through both the small _cog_ icon in the upper-right corner or by right-clicking on the name of the Component. Every Component's context menu contains a **Reset** option, which resets all values back to their default, sparing us from having to reset values manually. This is useful when working with `Transform` Components, as this option will set the object's position and rotation to `(0, 0, 0)` and its scale to `(1, 1, 1)`.