Let's create a new image that can be chosen by the editor of the page to be displayed above the navigation. First of all, create two images having a width of 800 pixels and height of about 100 pixels. Name them pageimage1.jpg and pageimage2.jpg. Upload the images with **Image Manager** to CMS Made Simple. Do not upload them to subfolders, but directly to the start folder of **Image Manager** (`uploads/images`).

In the admin console, open any page (**Content | Pages**). Choose the **Options** tab, and in the **Image** field, select the picture you would like to associate with this page, as shown in the following screenshot:

![](bbox=[0.26, 0.318, 0.745, 0.465])

Do the same with other pages and assign other images to them.

> If you cannot see the image in the drop-down field, then it is not uploaded into the right folder. Page images have to be found in **Content | Image Manager** or `/uploads/images` (not subfolder) on the web server.

Then add the image to the template. Open the **Business World** template (**Layout | Templates**), and add the page image to display it:

```
<!DOCTYPE html>
<html>
<head>
<title>{title} - {sitename}</title>
{stylesheet}
{metadata}
<meta name="description" content="" />
</head>
<body>
<div id="container">
<div id="header">
businessWorld
</div>
```