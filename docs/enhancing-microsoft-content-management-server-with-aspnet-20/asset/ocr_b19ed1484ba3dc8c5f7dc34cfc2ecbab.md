# # Visual Studio Templates

The installation of MCMS Service Pack 2 automatically registers the MCMS
developer tools such as MCMS Template Explorer in Visual Studio 2005. However,
before we can create MCMS applications with Visual Studio, we need to make the
website and item templates available.

+   1. Select Start | All Programs | Microsoft Visual Studio 2005 | Visual Studio
+   Tools | Visual Studio 2005 Command Prompt.

+   2. Execute the following commands, replacing MCMS_INSTALL_PATH with the
+   install location of MCMS (usually c:\Program Files\Microsoft Content
+   Management Server) and PATH_TO_MY_DOCUMENTS_FOLDER with the location of your
+   My Documents folder:
+   xcopy "MCMS_INSTALL_PATH\DevTools\NewProjectWizards80\Visual Web
+   Developer" "PATH_TO_MY_DOCUMENTS_FOLDER\Visual Studio 2005\
+   Templates\ProjectTemplates\Visual Web Developer" /E
+   xcopy "MCMS_INSTALL_PATH\DevTools\NewItemWizards80\Visual Web
+   Developer" "PATH_TO_MY_DOCUMENTS_FOLDER\Visual Studio 2005\
+   Templates\ItemTemplates\Visual Web Developer" /E

+   3. Execute the following command to register the templates with Visual
+   Studio 2005:
+   devenv /setup

+   4. Close the command prompt.

This completes the steps to upgrade to SP2, and our environment is now ready for
development! We can test our installation by viewing the version number in the SCA,
connecting with Site Manager, or by using the Web Author. Of course, any existing
MCMS web applications will at this time still be hosted by.NET Framework v1.1.

![](bbox=[0.2324, 0.743, 0.7715, 0.8156])