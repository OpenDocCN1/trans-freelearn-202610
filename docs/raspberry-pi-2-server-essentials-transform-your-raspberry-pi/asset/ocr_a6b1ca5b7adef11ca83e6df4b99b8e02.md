+   4. The final step will be to add a reference to the IoT extensions required for the Raspberry Pi specific features, such as GPIO.
+   5. Right-click on **References** | **Add Reference** | **Universal Windows** | **Extensions** and check **Windows IoT Extensions for the UWP**.

Windows is designed using the XAML mark up. If you have ever worked with Silverlight, then you should be familiar with XAML. XAML is a general purpose object instantiation language, which is mostly declarative, such as HTML, but with an XML twist. But no need to worry about the low-level mark up as Visual Studio allows you to design visually!

![](bbox=[0.25, 0.33, 0.3, 0.37])

> There is also a dedicated software called Blend that is purely used to create complex XAML layouts for applications that may require them.

+   1. In the **Solution Explorer** snap in Windows, locate and double-click on **MainPage.xaml**.
+   2. A new, blank designer view will open.
+   3. Select `40" IoT Device` from the top because we will be making this view for an IoT device with a large screen. There are options for smaller screens too if you are using a mini LCD perhaps. This helps you lay out your design within a perspective view. In advanced techniques, you can create separate views for various devices using separate XAML files. These will all scale as best as possible to the available screen.
+   4. You can now expand the **Toolbox** on the right-hand side and have a look under **XAML Controls**.
+   5. From the Toolbox, click and drag button control onto the white designer area. Then, drop it in **TextBlock**.
+   6. Resize the button control to be larger, as it may have dropped in at a really small size.
+   7. While the button control is selected, change your **Snap** in view (in the right) to **Properties**.
+   8. Find the **Common** tab and change the content value to change the text that is displayed on the button; for example, `Hello World!`
+   9. You can also change some properties for the text block, for example, the font size.