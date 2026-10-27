```
```html
<script src="@Url.Content("~/js/plugins/morris/morris.js")"></script>
<script src="@Url.Content("~/js/dashboard-demo.js")"></script>
}
```
```

+   12. Finally, open the _Layout.cshtml file and find the following line:
+   
+   ```html
+   <a href="index.html"><i class="fa fa-dashboard fa-fw"></i> Dashboard</a>
+   ```

+   13. Change the identified line to the following line:
+   
+   ```html
+   <a href="@Url.Action("Index","Home")"><i class="fa fa-dashboard fa-fw"></i> Dashboard</a>
+   ```

+   14. Save and close all files and run your project.

+   15. You should see a chart rendered, similar to the following screenshot, when you navigate to the **Dashboard** page.

![](bbox=[0.193, 0.401, 0.807, 0.819])