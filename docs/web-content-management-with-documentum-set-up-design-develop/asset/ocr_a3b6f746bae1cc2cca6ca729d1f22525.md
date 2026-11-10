![](bbox=[0.31, 0.15, 0.69, 0.24])

Figure 25.12: System prompt for commencing DocApp archive creation process

Refer to figure 25.12; click on OK, allowing the system to create an archive of your custom DocApp in the specified folder location. Usually the archive creation process takes a few minutes but can vary substantially if the number of objects included in your DocApp is very large. Archiving time can even go up to 5 hours for DocApps containing around 1000 objects.

Once the DocApp archive has been created, browse to the specified location on your machine and open the archive log file (<Your DocApp name>_ArchiveLog.html) to locate any system errors/warnings. It is advisable to look for warnings with the keyword unable to find out any serious failures.

Once you have the DocApp archive of the source Docbase, you can install it on the target Docbase via Documentum Application Installer. Before installing the archive, you may want to clean up the target Docbase and perform any pre-installation steps specific to your application.

It is better to set up Cabinets, Folders, Web Cabinets, Users/Groups, etc. in the target Docbase before you install the DocApp archive. If you want to avoid such manual activities, you can write pre-installation procedures and include them in the DocApp as we saw in step 3 earlier.

+   5. After the target Docbase is ready, you are all set to install the source Docbase DocApp archive over it. Set up the dmcl.ini file to point to the correct DocBroker host for the target Docbase.

Launch Documentum Application Installer from your machine and choose the target Docbase name from the Docbase dropdown (PN6549 Docbase in this example).

Provide Docbase user credentials for login authentication purposes and click on OK (refer to figure 25.13).

![](bbox=[0.28, 0.71, 0.72, 0.82])

Figure 25.13: Documentum Application Installer login screen