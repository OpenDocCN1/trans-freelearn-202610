When WAR files are deployed, they are unpacked by the container, which creates the structure shown in the following figure. The name of the top directory depends on the container in question. Tomcat, for instance, names the top directory after the WAR file. So, for instance, the following structure would be created by Tomcat upon unpacking a `deployedapp.war` file. As you can see, the internal structure of a WAR file is pretty much the same as the structure of your Eclipse dynamic web project:

![](bbox=[0.29, 0.25, 0.71, 0.55])

To create a WAR file from your project, right-click on the project and select **Export**. Select the WAR file option as shown in the next screenshot, specify the target directory through the button **Browse...** and hit **Finish**. Now you can deploy your WAR file to Tomcat by placing it under the `webapp` directory and restarting the container.

![](bbox=[0.3, 0.64, 0.7, 0.74])