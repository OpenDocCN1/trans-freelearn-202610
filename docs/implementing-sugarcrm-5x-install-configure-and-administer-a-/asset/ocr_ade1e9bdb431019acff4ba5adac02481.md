If it is missing, add it, save your changes, and then restart your web service and try again. If you encounter errors indicating that the extension cannot be found, it indicates that support for MS-SQL was not installed as part of your PHP installation. To resolve the problem, either reinstall PHP and make sure to install the MS-SQL extension, or locate the necessary DLL files and apply them to your PHP installation folder.

To restart your web service, select Start | Run. Type services.msc and click on OK. In the list that is displayed, right-click either on Apache2.2 or on World Wide Web Publishing (depending on whether you are using Apache or IIS for your web server), and choose Restart.

Locating the DLLs can prove a little trickier. A good starting spot is the main PHP website located at http://www.php.net/, but it may also require some general searching of the internet.

# # Installing SugarCRM on Internet Information Services (IIS)

IIS may or may not already be installed on your target server. One quick way to check whether it is or is not installed is to select Start | Control Panel | Administrative Tools. If you see an option for administering Internet Information Services, you are all set. If you do not see anything relating to IIS, you will need to install it through the Add/Remove Programs tool.

To install IIS, select Start | Control Panel | Add/Remove Programs and click on Add/Remove Windows Components. Windows will present a catalog of components that can be added to your installation of Windows. Select Internet Information Services and proceed with the remainder of the wizard until it finishes installing IIS for you.

Perhaps the biggest challenge involved with installing SugarCRM on IIS is configuring PHP.

In general, installing PHP will involve following the same instructions as those outlined earlier in this chapter in the section titled Installing PHP. However, you will want to choose IIS FastCGI at the Web Server Setup screen of the installation wizard. You may have to use the IIS ISAPI Module or IIS CGI option if you are using a version of IIS older than 7.0. Additional help on this topic can be found at the IIS PHP website, located at the following URL: http://php.iis.net/