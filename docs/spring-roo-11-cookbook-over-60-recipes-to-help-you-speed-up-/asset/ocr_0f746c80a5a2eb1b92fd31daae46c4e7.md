![](bbox=[0.234, 0.144, 0.762, 0.3])

9. As the screenshot shows, selecting the alt theme moves menu options to the right and the main content of the page is moved to the left. As mentioned earlier, theme selected by a user is saved in the browser cookie; therefore, if you close the browser and reopen it, you will find that the alt theme is applied by default on the web pages of the Flight-app application.

# How it works...

The perform package command runs maven's package command, which does the packaging of the project. The project is packaged as a WAR file because the packaging (as per the <packaging> element the in pom.xml file) specified for the flight-app project is war. It is important to note that when the perform package command is executed, tests are not executed. It executes maven's package command and specifies that the tests are skipped, as shown here:

```
..ch04-recipe>mvn package -Dmaven.test.skip
```

As mentioned earlier, the perform command of Roo can be used to execute maven commands. You can execute maven's package command (and execute tests) from the Roo shell, as shown here:

```
.. roo> perform command --mavenCommand package
```

> **Don't run an embedded Tomcat instance from a Roo shell**
> 
> You can run the flight-app project as a web application in the embedded Tomcat instance using Roo's perform command, but you should not do it because it will result in the creation of a different process, which you can't stop using CTRL+C and also you can't execute Roo commands from the Roo shell.

The flight-app project's pom.xml file configures Tomcat Maven Plugin (http://mojo.codehaus.org/tomcat-maven-plugin/index.html), which makes it possible to run the flight-app project as a dynamic web application in an embedded Tomcat instance. The following XML fragment shows the configuration of the Tomcat Maven Plugin: