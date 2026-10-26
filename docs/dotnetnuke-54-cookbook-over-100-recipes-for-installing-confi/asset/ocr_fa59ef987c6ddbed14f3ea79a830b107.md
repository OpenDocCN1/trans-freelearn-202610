# Basic Skinning

# How to do it...

+   1. Start by locating the default skin folder within your test DNN portal (`\Portals\_default\Skins`).
+   2. Next, create a new folder to hold the skin files. Call this folder `SampleHTMLSkin`.
+   3. Launch the Development Tool.
+   4. From the **File** menu, select **New File...**.
+   5. In the **New File** dialog, select **HTML Page** and click on **Open**.
+   6. This will give you a basic HTML file. Next, we need to add a simple HTML table to lay out our page. To create the layout shown in the sample diagram, our HTML table code should look like this:

```
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>

<link id="skin_css" href="skin.css" type="text/css"
      rel="stylesheet" />

    <title>Untitled Page</title>

</head>
<body>

        <table width="100%">
                <tr>
                        <td width="60%">
                                [MENU]
                        </td>
                        <td width="40%">
                                [USER]

                                &nbsp;&nbsp;&nbsp;&nbsp;
                                [LOGIN]
                        </td>
                </tr>
                <tr>
                        <td>
```
```