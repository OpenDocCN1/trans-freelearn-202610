+   9. Now scroll up to the top of Solution Explorer and right-click on the website. Select **Properties**.

![](bbox=[0.19, 0.134, 0.81, 0.385])

+   10. Select **Single startup project** from the radio control.
+   11. Select **Website** from the drop-down list.
+   12. Click on **OK**.
+   13. Now we can build the employee project and deploy it to the site. In the **Solution Explorer**, right-click on the Employee folder and select **Build**. You should see the **Build succeeded** message.
+   14. Follow the steps described in *Deploying a module as a standalone package* to create a ZIP file containing the Employee module.
+   15. Launch the site by looking under the Debug menu and selecting **Start Without Debugging** (or press Ctrl+F5).
+   16. All the source code for the site will now compile and the site will launch in your browser. After a pause you should see the Welcome screen.
+   17. Log into the test site as host (`syshost` in this examples).
+   18. Follow the steps described in *Deploying a module as a standalone package* to install the new ZIP file.
+   19. The ACME Employee module should now appear in the Control Panel drop-down list. Create a new blank page and add the Employee module to it.

> When you create the new page give access to All Users. Then you can debug your module without having to log in every time.