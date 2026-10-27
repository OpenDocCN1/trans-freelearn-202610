# Debugging web scripts

You have completed the development of your web scripts, deployed them successfully, and even registered your web scripts to the web script engine. Now, while testing the web script functionalities, you found that the logic you have included in the web script controller is not working as expected. Hence, your web scripts are failing to return the required response. The next thing you want to find out is what went wrong and why they are not working the way they should. It's now time to debug to identify the issue that will give you hints to fix the issue. We will now take a look at how to enable logging, how to remote debug at the server, and also some useful debugging techniques.

# Enable logging

Enabling the logging feature would be of great help while debugging an issue. Let's understand how to use logging in the case of both Java- and JavaScript-backed web scripts. The most important file you should remember is the log4j.properties file, which is available at the tomcat\webapps\alfresco\WEB-INF\classes location inside your Alfresco installation directory.

For debugging Java-backed web scripts, we need to perform the following steps:

+   1. You have the option to add org.apache.log4j.Logger statements in your Java implementation of the controller.
+   2. Then, you need to have the entry of your Java class added in the log4j.properties file.
+   3. For example, you have the Java class for your web script as com.example.TestWebScript. After adding the appropriate logger statements to the Java controller class, you should make an entry for the Java controller in the log4j.properties file as log4j.logger.com.example.TestWebScript=debug.
+   4. Restart the server and then hit your web script; you should be able to see the logger statements from the Java controller of the web script in the catalina logs.

For debugging JavaScript-backed web scripts, you will have to leverage the logger root object as follows:

+   1. You should add the required logger statements using the logger.log method in the JavaScript controller so that you can get enough details when you are looking at the logs.