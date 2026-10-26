# Advanced Skinning

```
```xml
<skinFile>
    <path>css</path>
    <name>med_font.css</name>
</skinFile>
<skinFile>
    <path>css</path>
    <name>large_font.css</name>
</skinFile>
</skinFiles>
```
```

+   18. Select **Save All** from the **File** menu to save your changes.
+   19. To see how the new skin will look on the site, log in as the SuperUser.
+   20. Deploy the skin from the skin folder to the DNN portal as described in the recipe _Deploying your skins and containers_. Be sure to include the new `/css` folder in the ZIP file.
+   21. Once the skin is deployed, look under the **Admin** menu and select **Skins**.
+   22. Select the **SampleASCXSkin** from the **Skins:** drop-down list and click on the Preview link.
+   23. As we placed the Stylesheet widget in the left pane, you will see three buttons have appeared. Clicking on the buttons will switch between the small, medium, and large font stylesheets we created. This causes the fonts on the page to dynamically change size.

![](bbox=[0.146, 0.534, 0.855, 0.768])

# There's more...

In this example we used extremely simple stylesheets, doing nothing more than changing font sizes. But the power of this widget is limited only by your creativity in your stylesheet code. You can experiment with other CSS elements and more CSS files to really see all the possibilities of this widget.