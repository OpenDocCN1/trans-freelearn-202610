If you are experiencing problems, you can remove the plugin from Windows | Control Panel and test Revit without the plugin:

![](bbox=[0.13, 0.135, 0.86, 0.375])

Figure 9.7 – Windows Control Panel option to remove plugins

If the plugin removal is not successful, or the plugin does not appear in the list to be removed, we need to uninstall the plugin manually.

Removing plugins manually can be challenging as you need to find them in multiple locations in your C:\hard drive.

We can find Revit plugins installed in the following paths:

+   - C:\ProgramData\Autodesk\Revit\Addins\
+   - C:\Users\%Username%\AppData\Roaming\Autodesk\Revit\Addins\
+   - C:\ProgramData\Autodesk\ApplicationPlugins
+   - C:\Program Files\Autodesk\Revit 2021\AddIns

> Note
> You may need to select the folder based on the Revit version.

Once we find the required plugin, we can select and delete the files to remove from Revit.

If you wish to test Revit without the plugin, you can move the plugin files to another folder and restart Revit. After testing, you can move the plugin back to the same folder.