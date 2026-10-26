
<name>vendor3.gif</name>
</skinFile>
<skinFile>
    <path>images</path>
    <name>vendor4.gif</name>
</skinFile>
<skinFile>
    <path>images</path>
    <name>vendor5.gif</name>
</skinFile>
<skinFile>
    <path>images</path>
    <name>vendor6.gif</name>
</skinFile>
</skinFiles>

+   12. Select **Save All** from the **File** menu to save your changes.
+   13. To see how the new skin will look on our site, log in as the SuperUser.
+   14. Deploy the skin from the skin folder to the DNN portal as described in the recipe *Deploying your skins and containers*. Be sure to include the new `/images` folder in the ZIP file.
+   15. Once the skin is deployed, look under the **Admin** menu and select **Skins**.
+   16. Select the **SampleASCXSkin** from the **Skins:** drop-down list and click on the **Preview** link.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_393_0.png)

17. As we placed the Rotator widget in the left pane, on the left side of the page you will see the first image. After a moment, the image will change as the rotation begins. We picked an `interval` value of `3000` so the image should change every 3 seconds. We chose `slide` as the transition and `left` as the direction, so the next image should slide in from the right side to the left.

## There's more...

You can experiment with different parameter values and see the results. For example, changing the transition to `snap` will have the images immediately replace each other without the slide animation and using a longer interval is helpful when the images are larger.

## See also

If you look closely you will see the ImageURL contains a reference to the `<%=SkinPath %>` variable. To learn more about other portal variables usable in skins, see the recipe *Showing portal settings in a skin* at the end of this chapter.

## Using the Visibility widget

In this recipe we will take a look at the Visibility skin widget. The Visibility widget creates a button on your skin that dynamically toggles the visibility of an HTML element without calling the server.

> As we are using JavaScript behind the scenes, the target element must be accessible in the Document Object Module (DOM). The DOM is an easy way to reference elements in an HTML form (like the DIV element). You can learn more at http://www.w3.org/DOM/.

To demonstrate the widget we'll modify the sample ASCX skin from the recipe *Creating a simple ASCX skin*, adding `BREADCRUMB` and `TEXT` controls inside a `DIV` element, then redeploying the skin to see the change.

## Getting ready

To follow along with this recipe you must have completed the following recipe from *Chapter 8*, *Basic Skinning*:

-   ▸ *Creating a simple ASCX skin*

## How to do it...

-   1. Find the folder called `SampleASCXSkin` that was created in the recipe *Creating a simple ASCX skin*.
-   2. Launch the Development Tool.
-   3. Select **Open File...** from the **File** menu.
-   4. Browse to the `SampleASCXSkin` folder and open the `SampleASCXSkin.ascx` file.

5. We're going to add the BREADCRUMB and TEXT controls to our skin so begin by adding the following registers at the top of the file:

```
<%@ Register TagPrefix="dnn" TagName="USER"
    Src="~/Admin/Skins/User.ascx" %>
<%@ Register TagPrefix="dnn" TagName="LOGIN"
    Src="~/Admin/Skins/Login.ascx" %>
<%@ Register TagPrefix="dnn" TagName="TEXT"
    Src="~/Admin/Skins/Text.ascx" %>
<%@ Register TagPrefix="dnn" TagName="BREADCRUMB"
    Src="~/Admin/Skins/BreadCrumb.ascx" %>
```

6. Next, scroll down to the HTML table holding the different panes and add the following code to the ContentPane:

```
<div class="content">
<table width="100%" border="0" cellspacing="0" cellpadding="0">
    <tr>
        <td valign="top" id="LeftPane" class="LeftPane"
            runat="server" visible="false">
        </td>
        <td valign="top" id="ContentPane" class="ContentPane"
            runat="server" visible="false">

<object id="VisibiltyButton" codetype="dotnetnuke/client"
            codebase="VisibilityWidget" declare="declare">
            <param name="title" value="Hide/Show Breadcrumbs" />
            <param name="collapseClassName" value="collapse_icon" />
            <param name="expandClassName" value="expand_icon" />
            <param name="targetElementID" value="testElement" />
        </object>

        <div id="testElement">
            <dnn:TEXT runat="server" id="dnnTEXT"
                CssClass="breadcrumb_text" Text="You are here >"
                ResourceKey="Breadcrumb" />&nbsp;<span>
            <dnn:BREADCRUMB runat="server" id="dnnBREADCRUMB"
                CssClass="Breadcrumb" RootLevel="0"
                Separator="&nbsp;&nbsp;" />
        </div>

        </td>
        <td valign="top" id="RightPane" class="RightPane"
            runat="server" visible="false">
        </td>
    </tr>
</table>
</div>
```

-   7. This gives us a text prompt, a breadcrumb, and the Visibility widget. What we need now is a little button to click on. To display a hide/show image on the button we must define the images in the `skin.css` file. Open the `skin.css` file.

-   8. Scroll down and add the follow style elements just after **Control Styles:**

```
/* Control Styles */
#login_style                 {float: right; padding: 10px 17px 0px 10px;}

/* Visibility widget */

.collapse_icon,
.expand_icon
{
    width: 14px;
    height: 14px;
    float: right;
    margin-top: 5px;
    background-repeat: no-repeat;
    cursor: pointer;
    border: 0;
    background-color: Transparent;
}

.collapse_icon
{
    background-image: url("/dotnetnuke/images/minus2.gif");
}

.expand_icon
{
    background-image: url("/dotnetnuke/images/plus2.gif");
}
```

-   9. This creates a small icon button that will show a plus or minus sign that changes as the visibility toggles. For simplicity we'll use some icons that come with DNN.

-   10. Finally, select **Save All** from the **File** menu to save your changes.

-   11. To see how the new skin will look on the site, log in as the SuperUser.

-   12. Deploy the skin from the skin folder to the DNN portal as described in the recipe *Deploying your skins and containers*.

-   13. Once the skin is deployed, look under the **Admin** menu and select **Skins**.

14. Select the **SampleASCXSkin** from the **Skins:** drop-down list and click on the Preview link.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_397_0.png)

15. A small minus sign icon will appear next to the breadcrumb we added. If you click on the minus sign, the breadcrumb will disappear and a plus sign icon will remain:

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_397_1.png)

16. Clicking on the plus sign icon will restore the breadcrumb.

## How it works...

This is a handy trick to save "real estate" on your pages by hiding large elements that do not always need to be visible. As the widget operates on the client side it is a very quick and efficient way to show and hide DOM elements.

## There's more...

In this example we used some common DNN plus and minus icon images, but you are free to create your own images and add them to the skin. Just change the path in the CSS file to point to your images and add the images to the skin manifest.

## Using the Stylesheet widget

Just as the Rotator widget can dynamically cycle through a set of images on the client side, the Stylesheet skin widget lets you dynamically switch between different stylesheets for your skin. This can be a very powerful tool for changing the entire appearance of your DNN pages on the client side with very little coding.

In this recipe we will create three simple stylesheets specifying font sizes (small, medium, and large) then add the Stylesheet widget to switch between them.

## Getting ready

To follow along with this recipe you must have completed the following recipe from *Chapter 8, Basic Skinning:*

-   - Creating a simple ASCX skin

## How to do it...

-   1. Find the folder called `SampleASCXSkin` that was created in the recipe *Creating a simple ASCX skin*.
-   2. Create a new folder within called `/css`.
-   3. To show switching between different stylesheets, we need to create three new CSS files and put them in this folder. Start by launching the Development Tool.
-   4. Then select **New File...** from the **File** menu.
-   5. At the **New File** dialog, select **Text File** and click **Open**.
-   6. Put the following code in the file:

```
```css
*
{
    font-size: 8pt;
}
```
```

-   7. Save the file as `small_font.css` in the `/css` folder.
-   8. Create another text file and add the following code:

```
```css
*
{
    font-size: 14pt;
}
```
```

-   9. Save the file as `med_font.css` in the `/css` folder.
-   10. Create one more text file and add the following code:

```
```css
*
{
    font-size: 28pt;
}
```
```

-   11. Save the file as `large_font.css` in the `/css` folder.
-   12. The `/css` folder should now look like this:

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_398_0.png)
