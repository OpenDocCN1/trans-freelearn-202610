# Building, Debugging, and Deploying Modules

+   6. Once the site loads, build the entire site by looking under the **Debug** menu and selecting **Start Without Debugging** (or press Ctrl+F5).
+   
+   7. All the source code for the site will compile and the site will launch in your browser.

> If you encounter an error on the build, you might need to add an additional reference. For example, in DNN 5.02 source, it was necessary to add a reference to `\Library\Components\Telerik\Telerik.Web.UI.dll` before building.

+   8. If this is the first time building the site, then it will go through the DNN installation process, just as it did in the recipe *Running the DotNetNuke installation* back in *Chapter 1*. However, this time we will create a new database just for testing our modules.
+   
+   9. At the first screen it will prompt for installation type. Choose **Typical** and click on **Next**.
+   
+   10. The next page will test the file permissions. Click on **Test Permissions**. After a pause you should see the message **Your site passed the permissions check.** Click on Next.
+   
+   11. On the next page we choose the database. For debugging we will create a new database NOT an existing DNN database. Select **SQL Server 2005/2008 (Express) File** in the radio button at the top of the page.

![](bbox=[0.191, 0.504, 0.809, 0.903])