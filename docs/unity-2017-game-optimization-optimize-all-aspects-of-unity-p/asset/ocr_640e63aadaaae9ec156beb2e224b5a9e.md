# # Scripting tips

The following tips are useful features to know when scripting.

# # General

We can modify various templates of new script, Shader, and Compute Shader files. This can be helpful to remove the empty Update stubs, which as we covered in Chapter 2, Scripting Strategies, can cause unnecessary runtime overhead. These files can be found in the following locations:

+   * Windows: `<Unity install>\Editor\Data\Resources\ScriptTemplates\`
+   * MacOS:
+     `/Applications/Unity/Editor/Data/Resources/ScriptTemplates/`

All settings on the object at the time of the copy will be kept. The same can be done with individual values of entire Components using the **Copy Component** and **Paste Component** options in the Component's context menu. However, the clipboard can only contain data for one GameObject, Component, or value at a time.

Another approach, which allows us to save the data of multiple objects during **Play Mode**, is to create Prefabs from them by dragging and dropping them into the **Project** window at runtime once we're happy with the settings. If the original object was derived from a Prefab, and we wish to update it across all instances, then we only need to overwrite the old Prefab with the new one we created by dragging and dropping the copy on top of the original. Note that this also works while **Play Mode** is active, but it can be dangerous since there is no dialog popup to confirm the overwrite. Be very careful not to overwrite the wrong Prefab.

We can use the **Frame Skip** button (the button to the right of the **Pause** button in the Editor) to iterate one frame at a time. This can be useful for watching frame-by-frame physics or Gameplay behavior. Keep in mind that this causes both one Fixed Update and one Update to be called per iteration, in equal counts, which may not exactly reflect actual runtime behavior where we tend to have an unequal number of calls to these callbacks.

If the **Pause** button is enabled when **Play Mode** begins, then the game will be paused just after the very first frame, giving us a chance to observe any anomalies that occurred during initialization of our Scene.