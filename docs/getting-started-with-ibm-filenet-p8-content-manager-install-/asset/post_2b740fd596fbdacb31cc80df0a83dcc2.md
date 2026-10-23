
For the sake of discussion, we'll assume you have chosen duckrqb.jar, the same name as the pre-made JAR file that we've provided for you. Your exported JAR file should also have the same contents as ours.

## New code module

You have a choice of installing the JAR into a CE repository as a code module or placing it onto the application server classpath. Many developers prefer the classpath approach because it seems less complicated. It does, however, mean restarting the application server for any changes, and that can be an annoyance during iterative development. We use the code module approach and suggest that you use it, too, since that is the approach that you will use in production.

Deploying a code module for a new subscription is fairly painless since FEM has a wizard that will lead you through the steps. For our case, right-click the Document class node in FEM and select Add Subscription to launch the wizard to add a synchronous handler for Creation Event and Update Event. Click the New button to create an Event Action, and you'll be prompted for the location of the class or JAR file and the fully-qualified name of the Java class: net.example.wjc.duck.DuckRQB. FEM will create all the needed linkages and will file the code module object into the Object Store's /CodeModules/ folder. The filing is for your convenience and is not a requirement for using a code module.

> You can also place a code module into the Object Store just like any other document, and you can file it or not, as you wish. If you do create a code module manually, be sure it is created as an instance of the CodeModule class or it won't be usable. See the product documentation for other code module considerations.

Right-click on the Restricted Quantity Satellite class node in FEM and select Add Subscription. This time, the event is Restricted Quantity Borrow/Return Event, and you should use the DuckRQB code module that we just created. The code module has conditional logic so that it can be used for multiple purposes. It will know what type of event is triggered and what the triggering object is.

## Modifying a code module

The process for modifying a code module, which you will surely do during development, is a bit more complicated because the internal linkages reference a specific document version as the code module. So, whether you create a completely new code module for each iteration or do a checkout/checkin cycle for a single code module's Version Series, the subscription linkage must be updated to point to it. We'll describe that process here.

Use FEM to locate your code module (probably in the /CodeModules/ folder). Perform a normal checkout/checkin cycle to update the content of your code module. For simplicity, we suggest that you do not use minor versions. FEM will now be displaying the latest version of the code module, but it is not yet referenced by anything. (If FEM is not displaying the correct major/minor version numbers, you may have to refresh the view of the /CodeModules/ folder.) Right-click on the code module and select Copy Object Reference. That puts a pointer to that specific object into a clipboard of sorts in FEM.

![](img/2295191c450a0c86b2d82df6cec04526_389_0.png)

Refer to the previous figure for the following steps:

1. Right-click the Restricted Quantity Satellite class node, select **Properties**, and click on the **Subscriptions** tab.
2. You will see the subscription you created. Select it and then click the **Properties** button. The subscription properties pop-up will appear. Click on the **Properties** tab to get the generic property sheet.
3. One of those properties will be Event Action. Click on its row in the **Property Value** column. The event action properties pop-up will appear. Click on the **Properties** tab to get the generic property sheet.
4. Select the **All Properties** filter choice. One of the properties in the list will be Code Module. If you click on its **Property Value** column, the code module properties pop-up for the earlier version of the code module will appear. That's not what we want. Instead, right-click on the **Property Value** column for that row and select **Paste Object**. After a confirmation dialog, the previously copied object reference (the one we copied earlier) will be used to populate that Code Module property value.

Repeat the preceding steps for the **Document** class. We'll be the first to admit that this process seems a little arcane and arbitrary. It comes across that way because FEM doesn't have specific helper panels for this, so we used the generic property sheets. Of course, revising a code module in production is a pretty rare event. After you've done this a few times, and if you've used logical names for the subscription, the event action, and the code module, we think you'll forget about its arbitrary nature and be able to click through it quickly.

Once you've updated a code module, you can delete older versions of the code module at your leisure. The CE will not let you delete a code module that is referenced by any subscription, so you don't need to worry about that aspect.

## Debug logging

Our DuckRQB code module class has some rudimentary debug logging built in. The debug logging, if enabled, is simply written to standard output. For our WAS CE, that ends up in the file /opt/ibm/WebSphere/AppServer/profiles/AppSrv02/logs/server1/SystemOut.log.

Logging is disabled by default. You can enable it by setting the Java system property DuckRQB.debugLog to the value **true**. In WAS, you can do that via the WAS console by navigating to **Servers | Server Types | WebSphere application servers | server1 | Java and Process Management | Process Definition | Java Virtual Machine**. In the text box for **Generic JVM Argument**, add **-DDuckRQB.debugLog=true**. To disable logging, either remove that item or change the value to **false**. In any case, you will probably have to restart the application server for the new value to take effect.

Remember, you are controlling debug logging for the event handler running inside the CE, so you want to do these manipulations on the CE instance of the application server.

## The Duck project

We did the implementation of the DUCK web application using **JavaServer Faces (JSF)**. This isn't the cutting edge technology that it was thought to be a few years ago, but it's still a fast way to create an application. It has reasonable component libraries, especially for the straightforward things we want to do, and it works with all of the application servers supported by CM. We'll leave it to you, if you are so inclined, to re-implement the application as an in-browser AJAX application or whatever else would make it look slick and modern in your enterprise's eyes.

We'll be using the popular open source integrated development environment Eclipse to create the application. We're using Eclipse 3.6.0 (**http://www.eclipse.org**; specifically, we're using the bundle called Eclipse IDE for Java EE Developers) and will be using the Apache MyFaces implementation of JSF (**http://myfaces.apache.org**). Although there is an Apache MyFaces release corresponding to JSF 2.0, we'll use the version corresponding to JSF 1.2 because our needs are simple and because there is a better guarantee that you will find support for it in your environment. For example, it's possible to install and use JSF 2.0 with WAS 7, but it has JSF 1.2 out of the box.

The downloadable material for this chapter includes complete Eclipse projects as ZIP files. You can put a ZIP file in any convenient location (it is not necessary to unZIP it) and import it into your Eclipse workspace by navigating to **File | Import | General | Existing Projects into Workspace**. Choose **Select archive file** and browse to the ZIP file. The project will be imported as a new project in your Eclipse workspace. The project names will be the ones we used when we created them.
