# Granting access to modules

Once security roles are defined and assigned to users the last step is to decide which users can access the modules in the portal. You specify access to modules in two ways: a module can inherit access from the page where it is placed or you can turn off inheritance and specify directly on the module what security roles can access it. Some modules are available to all users while others are private. If you don't have the security role required by the module you will not see the module when the portal page is displayed.

# Getting ready

For this recipe you need a module. The default DNN portal includes a small HTML module called **Sponsors** that displays some of the companies that support DNN. We will take the normally private module and make it public so everyone can see it. Keep in mind that the recipe below will work with any module in DNN, not just with Sponsors.

# How to do it...

+   1. Begin by logging in as the Portal Administrator (`sysadmin` in our examples).
+   2. Look at the front page of the portal and scroll down to the bottom of the page. Look for the **Sponsors** or just use any module you see on the page.
+   3. Locate the module action menu. It's the small triangle next to the module name. Hover your mouse over the small triangle and the action menu will display.

![](bbox=[0.379, 0.558, 0.623, 0.782])