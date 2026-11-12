Click the **Launch Default Browser** button in the **GWT Development Mode** window to launch the `flightapp-gwt` application. If not already installed, you'll be prompted to install the **Google Web Toolkit Developer Plugin** for your browser, which is required when you are running a GWT application in development mode.

+   6. If you want to run the GWT application from your Eclipse IDE, then right-click the **flightapp-gwt** project in Eclipse IDE and select **Google | Web Toolkit Settings...** option. Select the **Web Application** option and check the option **This project has a WAR directory**, as shown here:

![](bbox=[0.24, 0.28, 0.76, 0.47])

As shown in the screenshot, set the **WAR directory** value to `src/main/webapp`—Maven's standard WAR directory that contains the `Application Scaffold.html` host page. Make sure that the **Launch and deploy from this directory** option is unchecked.

+   7. Right-click on the **flightapp-gwt** project in Eclipse IDE and select the **Run As | Web Application** option. Select `ApplicationScaffold.html` (or `index.html`) page in the **HTML Page Selection** dialog, as shown in the following figure:

![](bbox=[0.305, 0.615, 0.695, 0.79])