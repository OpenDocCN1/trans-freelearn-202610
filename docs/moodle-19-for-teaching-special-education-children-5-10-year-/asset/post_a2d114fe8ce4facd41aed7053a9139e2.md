
## 1

## Matching Pictures

> *When we create exercises for e-Learning platforms, the main focus is to present well-organized information for the students. However, children with special education needs require unique activities. They like computers and they usually enjoy visually rich and interactive activities because they allow them to focus on solving simple or complex exercises. Graphics, videos, sounds and toy-like hardware are their best friends when they need to improve their skills. It is necessary to combine multimedia resources with funny environments to allow them to enjoy their homework. They don't need boring exercises. Luckily, Moodle is an open e-Learning platform that is indeed useful to support the implementation of the most exciting activities for children with special education needs.*

We can combine Moodle, Web 2.0 tools, commercial and free software, and general purpose hardware devices to develop rich and interactive activities with pictures for children with **special education needs**.

In this chapter, Alice, a seven year old girl, goes to school. We will learn how to create activities related to her journey. By reading it and following the exercises we shall:

- Learn to create composite pictures
- Create activities to match composite pictures with sentences
- Search and organize 3D models to create a 3D scene
- Work with colors to define skill levels
- Create rich exercises to stimulate both the attention and the concentration
- Learn to take advantage of a gamepad and a digital pen to create funny exercises
- Add, run, and evaluate different kinds of rich and interactive activities

### Matching composite pictures

It was a very nice day. The sun was shining in the sky and the birds flew happily.

The school playground was full of children ready to begin their first day in school. Alice was among them.

The bell rang. Everyone had to go to class. All the children sat down and opened their schoolbags. Alice saw a lot of interesting things in the classroom. Shall we help her sort them out?

### Time for action – installing tools to manipulate 2D scalable clipart

We must first download and install some additional tools that will help us in searching, editing, combining, and converting 2D scalable clipart to the most appropriate file formats to use in Moodle and its related applications:

> The necessary tools will depend on the original 2D scalable clipart. However, we will be using some tools that will work fine with our examples.

1. Download the following files:

   | Application's name | Download link | File name | Description |
   | --- | --- | --- | --- |
   | Inkscape | http://inkscape.org/download/?lang=en | Inkscape-0.47win32.exe | This is a very complete free, open source, vector drawing program. This tool will enable us to import many vector assets in different popular file formats and export them to a format recognized by Moodle and Hot Potatoes. |
   | Microsoft Office Home and Student 2007 | http://www.microsoft.com/office/trial/default.aspx | X12-30107.exe | This is a commercial tool, but the trial offers a free fully functional version for 60 days. Microsoft Word 2007 will enable us to search vector assets in **Office Online Clip Art & Media**. |

2. Run the installers and follow the steps to complete the installation wizards.

3. Once Inkscape's installation is completed, you will be able to load and edit many vector assets in different file formats, as shown in the following screenshot:

![](img/a18a528f2a8e54d48b64e97eaf62e865_26_0.png)

4. Once Office 2007 installation is completed, you will be able to use the powerful clipart search capabilities offered by Word 2007, as shown in the next screenshot:

![](img/a18a528f2a8e54d48b64e97eaf62e865_26_1.png)

7. Click on the Go button. Word will begin downloading and displaying clipart related to the blackboard keyword, as shown in the next screenshot:

![](img/a18a528f2a8e54d48b64e97eaf62e865_28_0.png)

8. Position the mouse pointer over the desired clipart's thumbnail. Since you want to change the picture size without losing quality using Inkscape, make sure it is a WMF (Windows Meta-File) or an EMF (Enhanced Meta-File) file. Word displays the file format on the right-hand side of the tooltip that appears when you move the mouse pointer over each picture's thumbnail.

> WMF is an old scalable vector format. Neither Moodle nor Hot Potatoes offer direct support to WMF graphics. However, using the next steps, we can also combine any vector formats supported by Inkscape, such as SVG (Scalable Vector Graphics), AI (Adobe Illustrator), PDF (Adobe PDF), and EMF (Enhanced Meta-File), among others.

9. Right-click on the desired clipart's thumbnail and select **Preview/Properties** in the context menu that appears. Word will display a new dialog box showing a larger preview of the scalable clipart and a temporary file name, as shown in the next screenshot:

![](img/a18a528f2a8e54d48b64e97eaf62e865_29_0.png)

10. Triple-click on the long path and file name shown after **File**. This way, you will be sure that the temporary file's full path is selected. Then, right-click on it and select **Copy** in the context menu that appears.

11. Next activate Inkscape—remember that it was running minimized. You can use **Alt + Tab** or **Windows + Tab**. Don't close the clipart's preview window.

12. Select **File | Import** from the main menu. Click on the **Type a file name** button (the pencil with a paper sheet icon) and paste the previously copied temporary file's full path in the **Location** text box. The path is going to be similar to `C:\Users\Username\AppData\Local\Microsoft\Windows\Temporary Internet Files\Content.IE5\171CJB0C\MC03491330000[1].wmf`, as shown in the next screenshot:

14. Return to Word 2007, close the Preview/Properties dialog box and repeat the aforementioned steps (6 to 13) for each vector graphic to combine inside a box—Inkscape's drawing area. In this case, repeat those steps searching for an eraser. Now, two scalable clipart items will appear with different sizes in Inkscape's drawing area, as shown in the following screenshot:

![](img/a18a528f2a8e54d48b64e97eaf62e865_31_0.png)

15. Select File | Save from Inkscape's main menu. Save the file as image010101.svg in the previously created folder, C:\School.

16. Now, repeat the aforementioned steps (6 to 14) to create a new drawing composed of two vector graphics, a case and scissors. Two new scalable clipart items will appear with different sizes in Inkscape's drawing area, as shown in the next screenshot:

17. Select File | Save from Inkscape's main menu. Save the file as image010102.svg in the previously created folder, C:\School.

### What just happened?

We combined two pairs of 2D scalable clipart items in Inkscape's drawing areas. We searched for vector assets in **Office Online Clip Art & Media** and we imported the temporary files created by Word into Inkscape's drawing area.

We now have the following two boxes:

- image010101.svg: A blackboard and an eraser
- image010102.svg: A case and a pair of scissors

The exercise will look nice using these vector digital art assets.

> We used a naming convention for the pictures, as we want to keep everything well organized for our exercises. image010101 means image for chapter 01; exercise 01; picture number 01.

6. Select File | Save, to save the changes made to the original Inkscape SVG file.

7. Select File | Export Bitmap. A dialog box showing many export options will appear. Enter 30 on the first dpi (Dots Per Inch) textbox.

8. Click on the Drawing button, and then on Export. Inkscape will export the drawing in PNG (Portable Network Graphics) format. The exported bitmap graphics with a big blackboard and a big eraser will be saved in C:\School\image010101.png.

9. Next, open the other vector graphic file in Inkscape (C:\School\image010102.svg).

10. Repeat the aforementioned steps (2 to 7) to export a new bitmap graphic with a big case and a pair of big scissors, as shown in the next screenshot. The exported bitmap graphic will be C:\School\image010102.png.

![](img/a18a528f2a8e54d48b64e97eaf62e865_34_0.png)

11. Click on the case picture to select it and shrink it.

12. Click on the scissors picture to select it and shrink it to match the new smaller case's size.

13. Drag-and-drop the scissors to move them below the case, as shown in the next screenshot:

![](img/a18a528f2a8e54d48b64e97eaf62e865_34_1.png)
