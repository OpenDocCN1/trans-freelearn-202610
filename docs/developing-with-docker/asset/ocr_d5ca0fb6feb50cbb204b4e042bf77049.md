# Spotify's Maven Docker plugin

It's available as open source and you can get the sources and documentation from GitHub: https://github.com/spotify/docker-maven-plugin. You can use this plugin to create a Docker image with artifacts built from your Maven project. For example, the build process for a Java service can output a Docker image that runs the service.

The usage of the Spotify's docker-maven-plugin is pretty straightforward. You will just have to include it as a plugin in your build and provide a `dockerDirectory` value in the configuration. The plugin will expect the Dockerfile in this directory, so if you specify that directory, it must contain valid Dockerfile. You can also build the image without Dockerfile. In this case, you will need to provide additional plugin configuration, such as `baseImage` (it will be used in the `FROM` instruction-the starting point in every Dockerfile, as you remember from Chapter 6, *Creating Images*), `maintainer`, `cmd` and `entrypoint`.

The contents of the directory specified as the `dockerDirectory` value will be copied into `${project.build.directory}/docker`. This will be the build context when the `docker build` command starts. Using the `resource` element, you can include additional artifacts to the Docker's build context. Take a look at the following example fragment of the `pom.xml` build file:

```
```xml
<build>
    <plugins>
        <plugin>
            <groupId>com.spotify</groupId>
            <artifactId>docker-maven-plugin</artifactId>
            <version>0.4.13</version>
            <configuration>
                <imageName>myImage</imageName>
                <dockerDirectory>docker</dockerDirectory>
                <resources>
                    <resource>
                        <targetPath/>
                        <directory>${project.build.directory}</directory>
                        <include>${project.build.finalName}.jar</include>
                    </resource>
                </resources>
            </configuration>
        </plugin>
    </plugins>
</build>
```
```