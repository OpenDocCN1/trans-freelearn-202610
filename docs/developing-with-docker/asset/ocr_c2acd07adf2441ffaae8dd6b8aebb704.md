To expose ports, you will need to define <port> sub-elements in the <ports> element. In the following example, we are exposing port number 8080 that we can map to the host's port later when running the container:

```
```xml
<ports>
  <port>8080</port>
</ports>
```
```

To define volumes, similar construction is needed and each <volume> element that is contained inside the <volumes> element will represent the volume. Exactly the same as the VOLUME instruction, it creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers.

```
```xml
<volumes>
  <volume>/path/to/volume</volume>
</volumes>
```
```

To define the entry point of the image, you will need, as you may have guessed already, an <entrypoint> element, as shown in the following example:

```
```xml
<entryPoint>
  <exec>
    <arg>java</arg>
    <arg>-jar</arg>
    <arg>/maven/myApplication.jar</arg>
  </exec>
</entryPoint>
```
```

The<entrypoint> specifies a command that will always be executed when the container starts, the same way the ENTRYPOINT instruction in the normal and traditional Dockerfile.

The<assembly> element in the build configuration specifies how build artifacts and other files your application may need will be put in the resulting Docker image. There are a couple of predefined assembly descriptors, such as artifact-with-dependencies (which will attach the project's artifact and all its dependencies into the image), artifact (where only the project's artifact will be put into the image, project (attaches the whole Maven project into the image), and rootWar (which will copy the artifact as ROOT.war into the exposed directory).