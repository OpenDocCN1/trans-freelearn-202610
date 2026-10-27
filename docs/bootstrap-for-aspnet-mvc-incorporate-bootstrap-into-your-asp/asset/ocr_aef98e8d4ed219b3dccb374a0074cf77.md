# # Creating the site Layout file

To maintain a persistent look across our site's pages, we'll use a Layout file. This layout file will use the basic Bootstrap HTML template at first and we'll build onto it as we progress throughout the book. To create this file, complete the following steps:

+   1. Create a new Layout file by right-clicking on the Views folder in the Visual Studio's **Solution Explorer** and navigate to **Add | New Folder**. Name the new folder Shared.
+   
+   2. Next, right-click on the Shared folder and navigate to **Add | New Item...** Select the **MVC 5 Layout Page (Razor)** item template, name the new item `_Layout.cshtml` and click on **Add**, as shown in the following screenshot:

![](bbox=[0.213, 0.348, 0.787, 0.684])

+   3. After the new file is added to you project, open it and replace its contents with the following HTML markup:

```
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
```
```