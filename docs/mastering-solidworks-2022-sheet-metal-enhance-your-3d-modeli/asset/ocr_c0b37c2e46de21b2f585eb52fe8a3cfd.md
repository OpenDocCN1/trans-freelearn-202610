![](bbox=[0.18, 0.09, 0.78, 0.49])

Figure 3.8: New Tab profile sketch

+   3. The simplest option may appear to be to create an **Extruded Boss/Base** feature using the **Features** tab. Selecting this tool shows that we now have the **Link to thickness** option, but we still need to ensure that the new section is extruded in the correct direction. Therefore, it is better to use a Sheet Metal-specific tool.
+   4. Instead of the **Extruded Boss/Base** tool, open the **Sheet Metal** tab or toolbar and select the **Base Flange/Tab** tool (this tool doubles up to create both **Base Flanges** and **Tabs**).
+   5. If needed, select the profile sketch that we just drew, then press **OK**.

A new tab section is now created and it is automatically linked to the thickness of the flange that it was made on.

**Note**
The feature is now displayed in the FeatureManager Design Tree as **Tab1**. This is because we are building upon an existing flange so that SolidWorks knows that the new profile should be a **Tab** rather than a new **Base Flange**.