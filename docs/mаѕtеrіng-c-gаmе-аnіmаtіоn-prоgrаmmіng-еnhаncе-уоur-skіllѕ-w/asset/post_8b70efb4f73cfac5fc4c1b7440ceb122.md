
In Figure 3.5, the effect of the deselection by using the null object is shown:

![](img/3cfb9832c943f5f7366fd104b5255971_133_0.png)

Figure 3.5: User interface is partially disabled when nothing is selected

No coordinate arrows are drawn at the feet of the model instance, as we hide the arrows when the null instance is selected. Also, the user interface is partially disabled, an effect of ignoring the null instance when counting the number of instances.

## Summary

In this chapter, we enhanced the instance selection methods in the code for better handling of finding the selected instance on the screen. Plus, we added the ability to select an instance by using the mouse. First, we implemented a button to center our virtual camera on the selected instance. Next, we added the ability to highlight the selected model, making it easier to spot on the screen. Then, we implemented a visual selection, allowing the user to select an instance by clicking on any instance with the mouse. Finally, we created the possibility to select no instance at all, avoiding accidental changes.

In the next chapter, we will lay some groundwork to adapt more game engine features to the model viewer. As well as splitting the behavior of the viewer into an edit mode and a pure viewer functionality, we will add the ability to revert ongoing changes to an instance. At the end of the next chapter, we will implement undo/redo functionality, allowing the user to revert changes, or to reapply previous changes.

## Practical sessions

There are some additions you could make to the code:

- Calculate azimuth and elevation when moving the camera.
  
  Right now, the values for azimuth and elevation are hard-coded. You could try to calculate both values from the transformation matrix.
- Implement a visual multi-selection.
  
  Enhance the selection so that holding Ctrl or Shift during selection clicks will add the newly selected instance to the other selected instances, instead of replacing the current selected instance.
- Extra difficulty: Implement instance moving via coordinate arrows.
  
  Since the coordinate arrows for the selection are drawn in a separate shader, you could try to add an extra set of selection indices to the arrows and adding the arrows to the selection texture. When the user clicks on one of the arrows instead of the mode, you could switch the application to a mode where the instance can be moved along the selected axis. This behavior is similar to moving an instance in any common 3D editor.

## Additional resources

- OpenGL basics: https://learnopengl.com/Getting-started/Hello-Triangle
- OpenGL Shader programming: https://learnopengl.com/Getting-started/Shaders
- Vulkan Tutorial: https://vulkan-tutorial.com
- Demystifying Floating Point Precision: https://blog.demofox.org/2017/11/21/floating-point-precision/
- Null object pattern: https://www.geeksforgeeks.org/null-object-design-pattern/

## Join our community on Discord

Join our community’s Discord space for discussions with the author and other readers:
https://packt.link/cppgameanimation

![](img/3cfb9832c943f5f7366fd104b5255971_135_0.png)

## 4

## Enhancing Application Handling

Welcome to Chapter 4! In the previous chapter, we added capabilities for a better selection of a single instance among a possibly large number of models and instances. We started with a simple “move to” function and added a highlight to the current instance as the next step. Then, we implemented a visual selection by using the mouse. Finally, we created a null object to allow selecting no instance at all.

In this chapter, we will focus on the edit mode. First, we will add the capability to switch off all controls and menus by creating a separate view mode. The split between edit and view modes will help us in later chapters to stop all automatic actions when we configure instance settings. Next, we will implement a simplified version of the undo functionality, allowing us to reset the settings of a model instance after applying changes. As the last step, we will implement undo and redo of setting changes on the instance level.

In this chapter, we will cover the following topics:

- Switching between edit and view modes
- Reverting changes before applying
- Implementing undo and redo functionality

## Technical requirements

The example source code for this chapter is in the chapter04 folder, in the 01_opengl_edit_view_mode subfolder for OpenGL and 02_vulkan_edit_view_mode for Vulkan.

After making instance selection easy in Chapter 3, changing the settings of an instance will be the next topic to tackle. Right now, we have a Control menu on the screen, capturing the mouse input if the pointer is placed over the ImGui window. Any change we make will remain permanent in the current application session. Reverting any accidental rotation or scaling to the exact same value as before could be very difficult since we would have to memorize the previous values to undo the change.

We will now change the application to toggle to a mode without any controls or selection and add a rather simple but useful undo and redo functionality to roll changes to instances back or forward.

## Switching between edit and view modes

If we start the current version of the application, we will see that a large part of the screen is blocked by the user interface whenever we want to make changes to instances.

Figure 4.1 shows what the user interface window looks like when all headers are expanded:

![](img/3cfb9832c943f5f7366fd104b5255971_140_0.png)

Figure 4.1: User interface blocking parts of the screen

As more and more options will be added later in the book, the user interface may fill up even more of the available window space. In addition, a selected instance will blink if the highlight is active and the coordinate arrows are placed below it.

To change the overall appearance, we must check which features should be disabled in a separate view mode.

## Deciding what should be switched in view mode

Our main goal for this section is to remove the ImGui Control window to have the full application available for interacting with the model instances. We could just switch off the rendering of the user interface, but if we do not show the user interface window, there is no need to do any calculations like the timer and FPS updates. So, for the first decision, we will skip the user interface in the View mode.

We must also add a shortcut to avoid forwarding mouse button events to ImGui when the user interface is not active. ImGui saves the positions of all control elements, and we would still get unwanted side effects, like being unable to move the camera if we switch the user interface while the mouse pointer is above the ImGui window.

Even without a user interface, all parts related to the instance selection are still active. This means we do not need the instance selection in view mode, since there is no way to adjust instance parameters without a user interface. Our second decision here is to switch off everything connected to the instance selection. We will disable mouse picking, including the selection shaders, the coordinate arrows, and the highlight calculation.

We will also adjust the window title, reflecting the current view/edit state of the application. Changing the window properties from the renderer requires a bit of callback “magic” but helps to immediately see what mode the application is in.

The implementation of most of the switch of view and edit mode is simple. We just need a variable to save the current state, and some of the branches to enable or disable above specified operations for the two modes.

## Adding the state variable plus code

As the first implementation step, we create a new enum class named appMode in the OGLRenderData.h file in the opengl1 folder:

```
enum class appMode {
    edit = 0,
}
```
