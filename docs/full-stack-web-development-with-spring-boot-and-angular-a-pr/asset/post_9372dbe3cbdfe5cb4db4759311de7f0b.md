
![](img/7c3ddd2e5893c9b5a0220148ed886548_395_0.png)

Figure 16.1 – The frontend folder inside the Spring Boot project

Figure 16.1 shows all the Angular files and folders inside the frontend folder, which is inside the Spring Boot project.

You can name the frontend folder whatever you want, so long as you map the path of the frontend folder to the workingDirectory property of Frontend Maven Plugin and the fileset property of Apache Maven AntRun Plugin, which allows you to run Ant tasks from within Maven.

Let's use the two Maven plugins, frontend-maven-plugin and maven-antrun-plugin, which we need in the next section.

## Using frontend-maven-plugin

In this section, we will use frontend-maven-plugin, which will install npm and Node.js locally. It will also run the npm build command in the frontend folder and copy the build files that npm build has generated.

So, let's start:

- 1. Go to your pom.xml file and insert the following code as one of the build plugins in your Maven pom file:

```
<plugin>
    <groupId>com.github.eirslett</groupId>
    <artifactId>frontend-maven-plugin</artifactId>
    <version>1.12.1</version>
    <configuration>
        <workingDirectory>frontend</workingDirectory>
        <installDirectory>target</installDirectory>
    </configuration>
...
// See full code on https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-16/superheroes
</plugin>
```

The preceding markup shows that, while building the Spring Boot application, the plugin will install Node.js v16.17.0 and npm CLI 8.19.1 in the frontend working directory. It will also execute the npm install command to download all dependency packages of the Angular application.

After performing all the necessary installations, the next thing that the plugin will execute is the npm run build command, which will create a production build of the Angular application.

- 2. Next, we must edit the npm build script in the package.json file of the Angular application using the following code:

```
"build": "ng build --configuration production",
```

The preceding code tells Angular that npm run build is for a production build.

3. We also have to edit the environment.prod.ts file inside the environment folder of Angular. Change the code to this:

```
export const environment = {
  production: true,
  apiURL: "http://localhost:8080/api/v1",
  authURL: "http://localhost:8080"
};
```

apiURL and authURL are just temporary. We will change them and use the real API URL and Auth URL properties in the real deployment of the application. We need to add the preceding code because we are using apiURL and authURL in our app for development, but we are missing the values for the production environment.

When the app builds for the production environment, the Angular app will collect the values from environment.prod.ts instead of using the environment.ts file.

Now, let's learn about maven-antrun-plugin and configure our .pom file again.

## Using maven-antrun-plugin

This section will use maven-antrun-plugin in the Spring Boot application. Open up your pom.xml file and insert the following code into one of the plugins in the build block of the markup. Put it below the markup of frontend-maven-plugin:

```
<plugin>
  <artifactId>maven-antrun-plugin</artifactId>
  <executions>
    <execution>
      <phase>generate-resources</phase>
      <configuration>
        <target>
          <copy todir="${project.build.directory}/classes/public">
            <fileset dir="${project.basedir}/frontend/dist/superheroes"/>
          </copy>
        </target>
      </configuration>
      <goals>
        <goal>run</goal>
```

```
</goals>
</execution>
</executions>
</plugin>
```

Here, maven-antrun-plugin is a configuration that copies the files and folders of the "${project.basedir}/frontend/dist/superheroes" path and pastes them into the todir="${project.build.directory}/classes/public" path before running the task. This will copy the frontend app and put it in the root folder of the JAR file of Spring Boot.

Now, let's configure the Spring MVC configuration of our app.

## Implementing WebMvcConfigurer

In this section, we will make the Spring Boot application a host for the Angular application by adding a configuration file. To do this, we must add a configuration class to the config directory of our Spring Boot application and name it MvcConfig.

After creating the MvcConfig class, add the WebMvcConfigurer interface to the file, like so:

```
@Configuration
public class MvcConfig implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
... //See full code on https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-16/superheroes
    }
}
```

The preceding code can also be found in the GitHub repository for this chapter.

The MvcConfig class implements WebMvcConfigurer and overrides the addResourceHandlers method.

Pass the "/**" argument into addResourceHandler, like so:

```
addResourceHandler("/**")
```

This will make the configuration affect all project routes. This part belongs to the external-facing URI of the application.

Then, map the external-facing URI path to the directory where the resources are located, like so:

```
addResourceLocations("classpath:/public/")
```

Finally, add new ClassPathResource ("/public/index.html"). This redirects requests that are not handled by the Spring Boot application that are going to the Angular application or the frontend application.

We are close to packaging both applications into one. We’ll learn how to do this in the next section.

## Packaging Spring Boot

In this section, we will package the Spring Boot application and the Angular application. The idea is to combine the two applications, giving you one single JAR file to deploy. Let’s learn how to do this.

After all the configurations we did, now, it’s a matter of running the following Maven commands in the same order:

```
mvn clean
mvn package
```

The mvn clean command cleans the Maven project by deleting the target directory, whereas the mvn package command builds the Maven project and creates an executable JAR file.

These two Maven commands are enough to create an executable JAR file with the Spring Boot and Angular packages inside it; see Figure 16.2:

![](img/7c3ddd2e5893c9b5a0220148ed886548_400_0.png)

Figure 16.2 – Target folder with the JAR file inside

Packaging the applications is simple since we already did all the hard work configuring the Angular and Spring Boot applications.

Now, we have the JAR file. In the next section, we will run the JAR file using a Java command to see if everything is fine.

## Running the JAR file

In this section, we will run the JAR file we have packaged and see that the Angular application communicates with the Spring Boot application. Follow these steps:

- 1. To run the application, you can either use the **Run** button via IntelliJ IDEA or use the following java command:

```
java -jar superheroes-0.0.1-SNAPSHOT.jar
```

The preceding Java CLI command will run an executable JAR file. In the CLI, you will see that the Tomcat web server has started on port 8080.

- 2. Go to http://localhost:8080; you will be redirected to http://localhost:8080/login, which consists of a login form. *Figure 16.3* shows this login form:

![](img/7c3ddd2e5893c9b5a0220148ed886548_401_0.png)

![](img/7c3ddd2e5893c9b5a0220148ed886548_401_1.png)

Figure 16.3 – Login form

3. Try to log in and navigate to the anti-heroes page, where you can create new heroes or villains using the form:

![](img/7c3ddd2e5893c9b5a0220148ed886548_402_0.png)

Figure 16.4 – Anti-heroes form

Figure 16.4 shows that everything is working on the anti-heroes form, from signing in to using CRUD operations.

With that, we have finished packaging the Spring Boot and Angular applications. Now, let’s summarize what we have learned.

## Summary

In this chapter, you learned that the frontend-maven-plugin and antrun-maven-plugin plugins can help you package your web client and Spring Boot application as one executable JAR file to make the deployment easy. You also learned how to run a JAR file on your local machine that helps you prepare your application for deployment.

In the next chapter, you will learn how to use GitHub Actions to prepare the applications for deployment. You will also learn how to use Heroku to create an instance of the database for the application and then deploy the application to Heroku.
