
|  |  |
|---|---|
|  | described in the PDF guide. |
| `build-res` | Includes folder for the Apache Ant scripts (http://ant.apache.org). |
| `lib` | The directory of libraries which makes up the Pentaho Reporting Engine. |
| `source` | The source files used to make up the reporting samples. |
| `sql` | The file-based HyperSQL database instance used with the samples. This is the exact `SampleData` schema introduced in the previous chapters. |
| `WebContent` | Web content information used by one of the examples (mainly `WEB-INF/web.xml`). |

Before going ahead with other concepts, take a look at the content of each folder to be familiar with the SDK structure and content. In the PDF guide mentioned earlier, you will find further information about the anatomy of the Pentaho Reporting SDK. Feel free to read the guide before continuing to read the following paragraphs.

## Using the included Eclipse project

As described in the PDF guide, if you use Eclipse or IntelliJ IDEA IDEs, you can use the Eclipse project included with the Pentaho Reporting SDK to work directly with the examples. In practice, select the unpacked Pentaho Reporting SDK directory as your workspace and everything should work. You can also launch the `Sample1.java` and `Sample2.java` example applications directly from the file browser in Eclipse.

We won't provide more than these details about the Eclipse project, as it is well covered in the guide. From another point of view, we prefer to spend our next sections on the most modern approach, using Apache Maven ([https://maven.apache.org](https://maven.apache.org)) as software project management and comprehension tool.

## Introducing the included examples

There are five interesting examples included in the Pentaho Reporting SDK. The examples are described in the PDF guide, together with an overview introducing the interaction with the Reporting Engine. All the source code described in the PDF guide is available in the sample folder, and the names of the files are strictly connected with the examples. Following are brief descriptions of each example for reference:

- `sample0`: The base class. The `AbstractReportGenerator` class is extended by `sample1` and `sample2`. It contains the basic logic that creates a report, leaving the details of input and output to the classes that extend it.
- `sample1`: Static report definition, JDBC input, PDF output. The simplest embedding scenario produces a static report (no user input regarding data source or query), with JDBC input from the Pentaho-supplied `SampleData` HyperSQL schema, and produces a PDF on the local filesystem.
- `sample2`: Static report definition, JDBC input, HTML output. This example produces a static report (no user input regarding data source or query), with JDBC input from the Pentaho-supplied `SampleData` HyperSQL schema, and produces an HTML file on the local filesystem.
- `sample3`: Dynamically generated, JDBC input, Swing output. `Sample3.java` generates the same report as created in `sample1.java`, but it uses a Swing helper class defined in the Reporting Engine to render the report in a Swing preview window.
- `sample4`: Dynamically generated, JDBC input, Java servlet output. `Sample4.java` is `HttpServlet` which generates an HTML report similar to `sample2`. This example assumes you have a Java application server, such as Tomcat or JBoss, installed, configured, running, and accessible to you.

Feel free to read the content of the PDF guide and the description of the five examples in detail, before continuing to read the content of the following paragraphs. It would be great if you could test the examples in your environment. As mentioned in the previous section, we will continue introducing a different approach using Apache Maven.

## Creating a web application using Pentaho Reporting and Maven

In this section, you'll learn how to create a web application from scratch, using the Pentaho Reporting Engine. This example is useful if you also want to understand how to embed the Pentaho Reporting Engine into an existing web application. In this example, in particular, you will see how to create a Java web application showing the Pentaho report you developed in Chapter 2, *Getting Started with Report Designer*. As you probably remember, we saw how to create a Pentaho report using the Report Design Wizard and we named it `my_first_report`.

Differently from the examples introduced in the Pentaho Reporting SDK, in this book we are going to use Apache Maven, the most used software project management and comprehension tool for Java.

To better describe the tasks and give you the opportunity to test the correct results, we are going to split the project into a few subtasks: one paragraph for each task, with source code and screenshots to be sure you can reproduce the example with success in your environment.

![](img/9337ac94537bf7e092e1ea6531dce8fd_125_0.png)

*In the GitHub repository available at https://github.com/fcorti/pentaho-8-reporting, you can find the source code described in the examples of this book. In the `Chapter 03 - Getting started with Reporting SDK/pentaho-reporting-web-app` you can find the source code of these examples in particular.*

## Prerequisites of your environment

In Chapter 2, *Getting Started with Report Designer*, we introduced the only prerequisite your environment should have: Sun's JDK Version 8. Considering that we are going to use Apache Maven, be sure that you have it installed in your development environment. The suggested version is 3 or more recent. To verify your installation, run the following command from the command line: **mvn --version**

If correctly installed, you will see something similar to what is shown in the following screenshot:

![](img/9337ac94537bf7e092e1ea6531dce8fd_126_0.png)

If you don't have it already, the installation of Apache Maven is a simple process of extracting the archive and adding the `bin` folder with the `mvn` command to `PATH`. For further details on how to install Apache Maven in your environment, check the official web page at https://maven.apache.org/install.html.

![](img/9337ac94537bf7e092e1ea6531dce8fd_126_1.png)

> *Another obvious requirement is the availability of an internet connection from your machine/environment. Apache Maven is based on the idea of public repositories where the commands get libraries, source code, and everything that is needed to compile and run your application. Be sure your laptop is connected to the internet during the execution of the following tasks.*

## Creating a web application in Java

Now that your development environment is correctly set up, the next step is creating a new Java web application. This particular task may not interest you if you already have an existing web application and want to include the Pentaho Engine with it. However, we prefer to cover the entire life cycle of the development for completeness, assuming you will try the example in your environment for better comprehension.

Let's start from the very beginning by creating a Java web application from scratch, and then add an initial page before the compilation and the first run.

## Creating a Java web application using Apache Maven

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
