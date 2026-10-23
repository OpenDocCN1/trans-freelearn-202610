
The filename for the DUCK web application Eclipse project is duck.project.ZIP. Import it into Eclipse as described just previously. In short order, the Duck project will appear in the Eclipse Project Explorer panel, as seen here:

![](img/2295191c450a0c86b2d82df6cec04526_392_0.png)

> You will also need references to Jace.jar and log4j.jar provided by the CE client installer. If your copies of those JARs happen to be in the same location as ours, then your project might be OK as-is. If they are in a different location, Eclipse will mention some build path errors to you, and you will need to correct the locations of those JAR files.

If you have not previously installed a JSF implementation package, you can easily download one from within Eclipse. Select the project name **Duck** in the **Project Explorer**, right-click and select **Properties**. In the pop-up dialog box, navigate to **Project Facets | Java Server Faces**. If you do not see an entry for JSF 1.2 (as in the following screenshot), click the download icon (the floppy disk with a down-pointing arrow).

![](img/2295191c450a0c86b2d82df6cec04526_393_0.png)

Select and download the Apache MyFaces implementation of JSF 1.2. Theoretically, any JSF 1.2 implementation should behave the same way as any other. We used Apache MyFaces for our sample DUCK implementation and suggest you do the same just in case practice doesn't match theory. Once it's downloaded, be sure the JSF 1.2 user library is selected and there is no selection for any other JSF implementation. The checkbox for **Include libraries with this application** should not be checked. That controls whether the JSF JARs are included in the WAR file that will be created. We won't need them since WAS 7 provides Apache MyFaces 1.2 automatically.

## Exporting and deploying Duck.war

If you have imported the Duck project into Eclipse and done any latent configuration fix-ups so that things build correctly, you can recreate Duck.war by exporting it from Eclipse. Select the Duck project in the Eclipse Project Explorer, right-click and navigate to Export | WAR file. During development, we like to include the source files in the exported WAR file, but you would probably not do that for your production deployment.

![](img/2295191c450a0c86b2d82df6cec04526_394_0.png)

The exported Duck.war will be the same as the one we included with the downloadable material for this chapter, with one exception. Our copy does not include Jace.jar and log4j.jar. If you decide to use our copy, you must add those two JAR files to the WEB-INF/lib/ directory inside the WAR file:

![](img/2295191c450a0c86b2d82df6cec04526_394_1.png)

## Deployment descriptor

Duck.war contains an XML deployment descriptor at WEB-INF/web.xml. If you have used the same server configurations as we used in the examples throughout this book, then you may be able to use that deployment descriptor without any changes. Most of it is standard JSF boilerplate, but there are a few context-param elements that are specifically for the configuration of the DUCK web application. If you want to change any of those values, you have a few choices:

- Application servers will sometimes let you directly modify those parameters for the deployed web application. This is usually not a very good choice except for temporary changes.
- You can modify WEB-INF/web.xml by extracting it from Duck.war, editing it, and replacing it in Duck.war.
- If you have the Duck project in Eclipse, you can use the Eclipse editor for web.xml before exporting the WAR file. This is probably the best choice since the changes are least likely to be lost.

All of the application-specific parameters have names starting with the prefix duck. If you modify anything in web.xml, be careful to not change anything other than the values for those parameters. Here is a complete list of parameters along with the values we have provided:

- `duck.ce.uri=iiop://wjc-rhel.example.net:2810/FileNet/Engine`: It gives the location of the target CE server. You should use the EJB transport form of this connection URI.
- `duck.ce.objectstore=OStore1`: It gives the name or Id of the Object Store that will be used for all operations.
- `duck.ce.showhiddenfolders=true`: It controls whether or not the DUCK UI will suppress folders whose Folder.IsHiddenContainer property value is true.
- `duck.ce.showhiddenproperties=true`: It controls whether or not the DUCK UI will suppress properties whose PropertyDescription.IsHidden property value is true.
- `duck.xt.baseurl=http://wjc-rhel.example.net:9080/WorkplaceXT`: It is a URL for connecting to the XT application. DUCK uses this when it constructs a URL for redirection to the GetContent servlet.

## Deploying to WAS

In keeping with our architectural tiers, we'll deploy DUCK to the same WAS profile that we used for XT since they are both web applications. This is not a requirement for the DUCK application, which merely needs to be able to connect to the configured CE server and the configured XT server.

> If you decide to deploy the DUCK application other than in the XT WAS instance, you will have to configure the WAS directory configuration and LTPA trust relationship using the procedure we described for XT deployment in Chapter 5, Installing Other Components.

Given Duck.war, prepared as just described, the first deployment into WAS is the routine adding of a new web application:

1. In the WAS console (for the XT WAS instance), navigate to Applications | Application Types | WebSphere enterprise applications. Click the Install button to launch the wizard for installing a new application.
2. When prompted, browse to your copy of Duck.war.
3. You can take defaults for things until prompted for the context root. Change the default value of / to /duck.
4. Complete the wizard and save the configuration. Duck_war will be listed among the Enterprise Applications but will not yet be started. WAS changes the period to an underscore when it creates the application name from the WAR file name.

You can use that same procedure for updating the DUCK application in place. All settings, including the context root, will be remembered and carried forward to the replacement WAR. There are, however, some additional configuration steps that must be done the first time DUCK is installed. Those are described in the following two subsections.

## Selecting the JSF Runtime

We want to make sure that we use the WAS-provided version of Apache MyFaces for maximum compatibility with our development environment (even though we did not intentionally use any non-standard JSF features).

From the Enterprise Applications screen, click on Duck_war and then click the link for JSP and JSF options. For JSF implementation, select MyFaces 1.2.

![](img/2295191c450a0c86b2d82df6cec04526_397_0.png)

## Configure role mapping

The DUCK application uses container-managed authentication via form-based login. The Duck.war deployment descriptor restricts access for all of the application screens to users who are members of the authenticated-duck-user role. Users not in that role will be automatically redirected to the enterprise login screen by the web container. We'll now map the authenticated-duck-user role so that all authenticated users are in that role.

From the Enterprise Applications screen, click on Duck_war and then click the link for Security role to user/group mapping. Check the box next to authenticated-duck-user, and, from the Map Special Subjects drop-down, select All Authenticated in Application's Realm.

![](img/2295191c450a0c86b2d82df6cec04526_397_1.png)

## Starting the application

After performing the above configuration steps and saving all settings, return to the Enterprise Applications screen, select checkbox for the Duck_war application, and click the Start button. The DUCK application should start successfully.

Log out of the WAS console and connect with your web browser to http://wjc-rhel.example.net:9080/duck/. You should be directed to the enterprise login screen. Log on with valid credentials (any of the LDAP directory users and passwords that you created in Chapter 2, Installing Environmental Components or later), and you should see the browse/query screen for the root directory of OStore1.
