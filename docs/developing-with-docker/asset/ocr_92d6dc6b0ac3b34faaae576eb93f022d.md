If you work on Linux or Mac, or perhaps have `curl` command installed also on a Windows PC, you can also execute it to see the following message:

```
```
curl http://localhost:8080
Greetings from Spring Boot in Docker!
```
```

That's it; our application works fine and it's time to put it in the container. We are going to use the fabric8.io Docker plugin for Maven and we already know it from the beginning of this chapter. We will use an existing Dockerfile approach, so let's create a `docker` directory in the `src/main` directory of our project and put the `Dockerfile` inside it. The `openjdk` official repository contains a lot of tags, each with different Java versions packaged for you to pick from. For example:

+   - 8u102-jdk, 8u102, 8-jdk, 8, jdk, latest
+   - 8u92-jdk-alpine, 8u92-alpine, 8-jdk-alpine, 8-alpine, jdk-alpine, alpine
+   - 8u102-jre, 8-jre, jre
+   - 8u92-jre-alpine, 8-jre-alpine, jre-alpine
+   - 9-b139-jdk, 9-b139, 9-jdk, 9
+   - 9-b139-jre, 9-jre

We are going to pick the latest one, which is Java `8u102-jdk`. We will take the inline configuration approach for our image build process, so the plugin section in the `pom.xml` will contain the instructions on how to build an image containing our application. You will need to add the following plugin definition to the `<build><plugins>` section of your `pom.xml` file:

```
```xml
<plugin>
    <groupId>io.fabric8</groupId>
    <artifactId>docker-maven-plugin</artifactId>
    <version>0.16.8</version>
    <configuration>
        <dockerHost>http://127.0.0.1:2375</dockerHost>
        <verbose>true</verbose>
        <images>
            <image>
                <name>hello-docker</name>
                <build>
                    <from>openjdk:8u102-jdk</from>
                    <ports>
                        <port>8080</port>
                    </ports>
                    <entryPoint>
                        <exec>
                            <args>java</args>
```
```