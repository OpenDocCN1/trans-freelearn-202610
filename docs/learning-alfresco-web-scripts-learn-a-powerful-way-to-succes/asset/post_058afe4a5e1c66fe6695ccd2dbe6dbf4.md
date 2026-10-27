
## Setting up your environment

Before we start, it is important to understand that Alfresco Maven SDK is compatible with Alfresco Community Versions 4.2 and above, and Alfresco Enterprise Versions 4.1.2 and above. It is interesting to know that, while using the Alfresco Maven SDK, you do not need to have Alfresco installed on your machine manually. The Alfresco Maven SDK will make the job easier for you and it will take care of downloading Alfresco on your machine based on the entries provided for version and groupId in the Project Object Model (POM) XML for your project.

Now, let's first make sure that we have the required prerequisites in place on our development machine before we begin creating a project using Alfresco Maven SDK:

- You need to have JDK 1.7 installed on your machine and configured properly. **Java Development Kit (JDK)** is basically a development environment to build applications using the Java programming language. You can download this from `http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html`. Once downloaded and installed, make sure to set up the JAVA_HOME environment variable and also the **Path** variable.

- You need to have Apache Maven 3.0.3 or above installed and configured properly. You can download this from `http://maven.apache.org/download.cgi`. You will also find the required instructions to install it based on the operating system you are using. Once downloaded and installed successfully, make sure that the environment variable M2E_HOME is set correctly and also the **Path** variable is modified accordingly.

Now, let's check whether Maven is referring to the correct JDK installation and ensure that you have installed the correct version of Maven by running the `mvn -version` command. On a Windows machine, running this command from the command prompt will display information about the Maven version, Maven home, Java version used by Maven, and the Java home:

```
C:\Users\ramesh.chauhan>mvn -version
Apache Maven 3.0.4 (r1232337; 2012-01-17 14:14:56+0530)
Maven home: D:\apache-maven-3.0.4\bin\..
Java version: 1.7.0_51, vendor: Oracle Corporation
Java home: C:\Program Files\Java\jdk1.7.0_51\jre
Default locale: en_US, platform encoding: Cp1252
OS name: "windows 7", version: "6.1", arch: "amd64", family: "windows"
```

## Exploring the Alfresco Maven repository

The Alfresco Maven repository is basically a common location where all the dependency JARs related to Alfresco are kept available, and you can configure your project to refer to the required dependencies from the repository in the pom.xml file of the project. You might want to explore the Maven repository for Alfresco that will be used by the Alfresco Maven SDK. Visit https://artifacts.alfresco.com/nexus/#view-repositories and explore the public release repositories that contain the released and stable artifacts. Also, the different available default project templates that are known as archetype in the terminology of Maven can be found under archetype-catalog available at https://artifacts.alfresco.com/nexus/service/local/repo_groups/public/content/archetype-catalog.xml.

## Creating the default project structure for AMP

In order to create the project structure for an AMP, we will use the project template provided by the Alfresco Maven SDK. This default project template is basically known as AMP archetype. This project template helps to have a standardized development, deployment, and the release approach for the Alfresco extensions you will develop for your project. The Alfresco AMP archetype can be used to create both repositories as well as share AMPs when required. In this chapter, we are going to use the AMP archetype for repository web scripts.

> The Alfresco Maven SDK also provides another useful archetype known as All-in-One archetype, which is basically a multi-module project and gives a complete installation of the Alfresco platform along with its components without requiring additional downloads. While exploring archetype-catalog.xml, you might have observed that there are a lot of archetypes available. The important archetypes, the Alfresco AMP archetype and All-in-One archetype can be identified having the org.alfresco.maven.archetype group ID in archetype-catalog.xml.

In order to create a default AMP project using the Alfresco AMP archetype, perform the following steps:

1. Run the following command from the console. Just to note here, if the https link does not work for you, then you can try using http instead of https in the following code snippet:

```
mvn archetype:generate -DarchetypeCatalog=https://artifacts.alfresco.com/nexus/content/groups/public/archetype-catalog.xml -Dfilter=org.alfresco.maven.archetype:
[INFO] Scanning for projects...
[INFO] Building Maven Stub Project (No POM) 1
[INFO] >>> maven-archetype-plugin:2.2:generate (default-cli) @ standalone-pom >>>
[INFO] <<< maven-archetype-plugin:2.2:generate (default-cli) @ standalone-pom <<<
[INFO] --- maven-archetype-plugin:2.2:generate (default-cli) @ standalone-pom ---
[INFO] Generating project in Interactive mode
[INFO] No archetype defined. Using maven-archetype-quickstart (org.apache.maven.archetypes:maven-archetype-quickstart:1.0)
```

2. You will be displayed with the available archetypes along with brief details about them, as shown in the following screenshot. You will be prompted to select an archetype. Type 1 and press Enter, as shown in the following screenshot:

```
Choose archetype:
1: https://artifacts.alfresco.com/nexus/content/groups/public/archetype-catalog.xml -> org.alfresco.maven.archetype:alfresco-amp-archetype [Sample project with full support for lifecycle and rapid development of AMPs (Alfresco Module Packages)]
2: https://artifacts.alfresco.com/nexus/content/groups/public/archetype-catalog.xml -> org.alfresco.maven.archetype:alfresco-allinone-archetype [Sample multi-module project for All-in-One development on the Alfresco platform. Includes modules for Repository, AMP, Share, Solr, Web Quick Start and embedded Jetty run]
Choose a number or apply filter (format: [groupId:artifactId, case sensitive contains]): : 1
```

3. In the next step, you will be asked to select the version of the archetype to have as a base for your project. This will list the available versions for the archetype you have selected. You can find these versions from the archetype-catalog.xml file as well. For the AMP archetype, 1.1.1 is the latest version at present. So, Type 5 and press Enter, as shown in the following screenshot:

```
Choose org.alfresco.maven.archetype:alfresco-amp-archetype version:
1: 1.0
2: 1.0.1
3: 1.0.2
4: 1.1.0
5: 1.1.1
Choose a number: 5: 5
```

4. Next, you will be prompted to provide the value for groupId, which you can consider as a package name in the terminology of Java. You can provide any name you like suitable for your project. Here, we will provide com.example as groupId, as shown in the following code:

```
Define value for property 'groupId': : com.example
```

7. Now, after some processing by Maven, it will display the **BUILD SUCCESS** message on the console and display the location of the project created from the archetype as an informational message, as shown in the following screenshot:

![](img/f02f759059b86af67e38b543d498a0d8_151_0.png)

It has generated a sample project structure for the Alfresco project as required to generate an AMP file. You can now go to the location where the default project structure has been created and explore it. Here project structure was created at the location `C:\Users\ramesh.chauhan\example-repo`. Inside the generated project structure, the most important file that you will find is `pom.xml` that is basically used for every Maven project to indicate to Maven about the project dependencies. Along with `pom.xml`, the generated project structure also contains the default configuration files and sample Java code with its supporting unit test and the configuration to run a local Alfresco instance for testing.

## Generating AMP from the default project

Once the default project structure is ready, let's try to generate the AMP file for it. Go to the location of the created project structure. In order to create an AMP file for it, run the `mvn package -DskipTests=true` command.

Here, we are not interested in running the tests while generating the AMP; hence, we have explicitly mentioned `skipTests` as true. If you want to run the test as well, you should simply execute the `mvn package` command.
