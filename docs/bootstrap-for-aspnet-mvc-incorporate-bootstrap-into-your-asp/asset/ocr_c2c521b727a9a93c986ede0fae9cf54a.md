# Adding charts to your views

In the SB Admin template, the Dashboard page contains a variety of attractive charts and graphs that you can use to display data in an interactive way to your users. We'll add the Dashboard page and its functionality that is included in the SB Admin template to our project by completing the following steps:

+   1. Create a new folder called morris inside the js\plugins folder.
+   2. Add the following two files from the SB Admin source files to the folder:
+      - raphael-2.1.0.min.js
+      - morris.js
+   3. Add the dashboard-demo.js file to the js folder.
+   4. Add the morris-0.4.3.min.css file to your project's css folder.
+   5. Next, open the _Layout.cshtml file and add the following code to the <head> element:
+      @RenderSection("styles",false)
+   6. By adding the preceding line, we can inject additional CSS styles into the layout page per view.
+   7. Open the Index.cshtml file inside the Views\Home folder.
+   8. Add the following code to the top of the file, just below the layout declaration:
+      @section styles{
+        <link href="@Url.Content("~/css//morris-0.4.3.min.css")" rel="stylesheet">
+      }
+   9. The preceding code will inject the styles for the morris chart component into our view.
+   10. Add the following markup to the view:
+       <div class="row">
+         <div class="col-lg-12">
+           <h1 class="page-header">Dashboard</h1>
+         </div>
+       </div>
+       <div class="row">
+         <div class="col-lg-12">
+           <div class="panel panel-default">