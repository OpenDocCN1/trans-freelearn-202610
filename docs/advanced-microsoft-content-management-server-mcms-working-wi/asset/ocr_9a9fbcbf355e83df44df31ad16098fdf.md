# Useful Placeholder Controls

One of the most exciting features of MCMS is the flexibility to create custom placeholder server controls. You can create placeholder controls of all shapes and sizes, providing myriad solutions tailored to the needs of authors in any organization. If something is not available out of the box—build it! And there is a good chance that you don’t even have to build custom placeholder controls from scratch. MCMS developers around the world may have already built a similar control in their own work. This chapter compiles a collection of commonly requested and very useful placeholder controls.

+   - **Date-time picker placeholder control**  
+     This provides a picker for authors to select a date and time value in the format of their choice. Say goodbye to badly formed and invalid dates with this nifty control.
+   
+   - **Placeholder control that accepts multiple attachments**  
+     This enables authors to upload as many files as they need by giving them a control that acts like multiple `SingleAttachmentPlaceholderControls` working in concert. We will also explore the undocumented Insert Attachment dialog that ships with Web Author and show how you can reuse it within custom placeholder controls.
+   
+   - **Image rotator placeholder control**  
+     Authors can manage web advertisements using this control that selects an image randomly from a specified resource gallery. A random image is displayed each time the control is loaded in presentation view.
+   
+   - **Placeholder control to store all kinds of HTML tags**  
+     If you have ever wanted to allow authors to upload restricted HTML tags and JavaScript, this control is for you. We show you the modifications required to get the `HtmlPlaceholderControl` to accept all HTML tags—including IFRAME, OBJECT, and APPLET tags and form elements!
+   
+   - **DataGrid placeholder control**  
+     This uses the familiar DataGrid to manage XML content. Authors can add, edit, and delete rows of XML content without needing to know the technical details behind it all. We also show how placeholders that demand a postback can be handled gracefully without incurring the leave warning alert.