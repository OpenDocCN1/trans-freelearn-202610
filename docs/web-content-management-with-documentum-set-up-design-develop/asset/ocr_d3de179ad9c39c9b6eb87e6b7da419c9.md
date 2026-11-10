If there are users connected to the target Docbase, the system will prompt you a message stating this. Click the Yes button to continue with the DocApp installation procedure.

Avoid any activities on the target Docbase while the DocApp installation process is going on. This is just to ensure that there are no inconsistencies while the system is overwriting existing objects or creating new objects in the target Docbase.

8. The DocApp installation process takes several minutes as we saw in the case of the DocApp archive creation process. It has been observed that with 1000 objects, the installation process can take more than an hour!

After the installation has been completed, click the Quit Installer button to exit the DocApp Installer application. Open the installation log file to check for errors or warnings during the installation procedure.

9. That's it! The DocApp has been installed on the target Docbase, which means that the specified objects in source Docbase have been migrated to the target Docbase. You now need to perform Web Publisher-specific tasks for your application to work correctly. For example, you might want to create Site Publishing Configurations in the target Docbase or make migrated workflows Available through the Web Publisher menu option Administration | Web Publisher Admin | Workflow Templates.

# 25.3 Deployment of Web Publisher Code and Custom Scripts

DocApp installation is just one part of migration. It simply takes care of Docbase objects; what about the WDK/DFC/other custom code you have written for you application or any scripts that you have customized for your business needs? How are these to be migrated?

Typically scripts/code files reside on two Documentum hosts:

+   - **Content Server:**
+     Scripts residing in the `%DM_HOME%\bin` directory
+     Example on a Windows machine: `C:\Documentum\product\5.2\bin`
+     Or
+     Java method code residing in `%DOCUMENTUM%\dba\java_methods`
+     Example on a Windows machine: `C:\Documentum\dba\java_methods`
+   - **Web Publisher host:**
+     Customized files residing in the following directories:
+     `\wp\custom`
+     `\wp\WEB-INF\classes`
+     `\wp\WEB-INF\tlds`