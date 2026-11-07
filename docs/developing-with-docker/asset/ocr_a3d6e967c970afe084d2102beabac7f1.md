```
<args>--jar</args>
<args>/maven/hello-docker-${project.version}.jar</args>
</exec>
</entryPoint>
<assembly>
<descriptorRef>artifact</descriptorRef>
</assembly>
</build>
</image>
</images>
</configuration>
</plugin>
```

Now, in order to compile, package, and build your Docker image, execute the following Maven command:

```
mvn clean package docker:build
```

In the output, the maven-docker plugin will output some details, such as specific steps from the process of building images. You will find all your EXPOSE, COPY, VOLUME, ENTRYPOINT instructions. All that you have configured either in the external Dockerfile or using the inline configuration in the pom.xml file itself. You can see the sample output in the following screenshot:

![](bbox=[0.1, 0.52, 0.9, 0.81])