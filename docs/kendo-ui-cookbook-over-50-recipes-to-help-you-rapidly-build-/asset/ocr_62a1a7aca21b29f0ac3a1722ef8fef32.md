# # 4 Kendo UI Editor

In this chapter, we will cover the following recipes:

+   - Creating a WYSIWYG Editor using the `kendoEditor` function
+   - Adding and removing tools in the Editor toolset
+   - Using the image browser tool to insert images into the Editor

# ## Introduction

The Kendo UI Editor widget allows you to create rich content using the **WYSIWYG (What-You -See-Is-What-You-Get)** interface. The interface allows you to format the text, create bulleted or numbered lists, and insert tables, hyperlinks, and images. The toolset can also be customized while initializing the Editor widget. It is also possible to create custom tools that can be used in the Editor. In addition to various Editor controls, you can use an image browser to insert images into the Editor.

# ## Creating a WYSIWYG Editor using the kendoEditor function

An Editor can be created either by using a `textarea` or `div` element. In this recipe, we will create an Editor using a `textarea` element and show you the default tools available for formatting the data.