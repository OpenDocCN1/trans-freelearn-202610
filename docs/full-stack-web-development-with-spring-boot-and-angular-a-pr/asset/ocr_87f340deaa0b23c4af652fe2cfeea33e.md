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

# Using maven-antrun-plugin

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