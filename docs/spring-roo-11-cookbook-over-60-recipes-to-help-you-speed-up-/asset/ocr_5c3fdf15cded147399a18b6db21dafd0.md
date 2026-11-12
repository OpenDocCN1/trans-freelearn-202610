The output shows that one of the files modified by Roo is `FlightDescriptionController_Roo_Controller_Finder.aj`. If you look at this file you'll find that Roo has added a method responsible for searching `Flight` entity instances. Now, take a look at this sentence from Chapter 4, Web Application Development with Spring Web MVC:

> It is important to note that in Spring Roo 1.1.3, the method responsible for searching entity instances is not created in `FlightDescriptionController_Roo_Controller_Finder.aj`.
> 
> This bug is resolved in Spring Roo 1.1.4 and above.

You can see that upgrading Roo to 1.1.5 automatically fixed the bug that existed in Roo 1.1.5.

# How it works...

We mentioned earlier that Roo is responsible for managing the AspectJ ITD files in your Roo project. So, when you upgrade to a later version of Roo, Roo takes care of modifying the code in the AspectJ ITD files of your project.

# There's more...

Roo's upgradation process is destructive, that is, you can't undo the changes made by Roo during the upgrade process. So, make sure that you keep a backup copy of your project before starting the Roo shell from the base directory of your project.