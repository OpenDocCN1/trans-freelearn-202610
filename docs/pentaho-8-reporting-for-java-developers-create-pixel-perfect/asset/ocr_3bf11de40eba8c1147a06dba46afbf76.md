# Creating a Java web application using Apache Maven

Creating a Java web application from scratch using Apache Maven is straightforward, thanks to the Maven Archetypes. In short, an Archetype is a Maven project template toolkit defined as an original pattern or model from which all other things of the same kind are made. For our needs, we are going to use `maven-archetype-webapp` to start a simple web application.

> *In this book, we won't detail the Maven Archetypes and we assume you are confident with the basics around their use and behavior. If not, don't worry, because we will describe a step-by-step approach. If you want to understand more, read the Apache Maven documentation at https://maven.apache.org/guides/introduction/introduction-to-archetypes.html.*

Back to our task, identify a folder in your machine to put in your new Java application (we will use `Desktop`). Open a Terminal, move into the working folder (in our case, `Desktop`), and execute the following command:

```
mvn archetype:generate
-DarchetypeArtifactId=maven-archetype-webapp
-DinteractiveMode=false
-DgroupId=com.example
-DartifactId=pentaho-reporting-web-app
```

Looking at the command, the `archetypeArtifactId` parameter specifies the Maven Archetype, the `interactiveMode` parameter tells Maven to ask (or not to use) questions of the user instead of defaults, the `groupId` parameter defines the root package of your Java source code, and the `artifactId` parameter defines the name of your web application.

> *Note that a brand new folder named `pentaho-reporting-web-app` is created. The new folder is going to contain the whole project, including configuration files, libraries, source code, and resources. This folder is the natural candidate to be stored in a version control repository (GitHub, SVN, and others).*