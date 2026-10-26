# How to do it...

+   1. Find the folder called `SampleASCXSkin` that was created in the recipe *Creating a simple ASCX skin*.
+   2. Create a new folder within called `/images`.
+   3. From the DNN installation, take any six of the sample vendor images from the `\dotnetnuke\Portals\0` folder and copy them into the `/images` folder.
+   4. Rename the image files `vendor1.gif`, `vendor2.gif` all the way to `vendor6.gif`. At this point the `/images` folder should contain the following images:

![](bbox=[0.273, 0.29, 0.727, 0.551])

+   5. Now launch the Development Tool.
+   6. Select **Open File...** from the **File** menu.
+   7. Browse to the `SampleASCXSkin` folder and open the `SampleASCXSkin.ascx` file.
+   8. Scroll down to the HTML table holding the different panes and add the following code to the `Left Pane`:

```
```html
<div class="content">
    <table width="100%" border="0" cellspacing="0" cellpadding="0">
        <tr>
            <td valign="top" id="LeftPane" class="LeftPane"
                runat="server" visible="false">

                <object id="ContentRotator" codeptype="dotnetnuke/client"
                        codebase="RotatorWidget" declare="declare">
                    <param name="imageCount" value="6" />
                    <param name="imageURL" value="<%=SkinPath %>images/" />
                    <param name="imageTemplate" value="vendor{INDEX}.gif" />
```
```