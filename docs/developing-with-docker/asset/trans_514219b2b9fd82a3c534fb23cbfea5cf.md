
在前面的示例中，我们已将插件包含在 pom.xml 文件的 plugins 部分。来自 ${project.build.directory} 的一个 jar 文件（默认情况下为 /target 目录）将被包含在构建上下文中。这次它将是一个以你的应用程序名称命名的 jar 文件，其中包含你构建的应用程序。

有了这种设置，再加上一个正确引用 /target 目录中应用程序 jar 的 Dockerfile，就足以从 Maven 的 pom.xml 创建 Docker 镜像。你只需执行以下 Maven 目标：

```
mvn clean package docker:build
```

如果你决定将生成的镜像推送到注册表，则需要额外的 -DpushImage 参数：

```
mvn clean package docker:build -DpushImage
```

如果你需要在仅运行 mvn deploy 时构建、标记并推送镜像，你需要将 Docker 命令绑定到 Maven 阶段，就像你通常对其他构建插件所做的那样。Spotify 的插件很不错、快速且简单。如果你只需要构建镜像，它可以作为一个起点。但它有一个限制：它不允许运行容器。由于该插件只能用于构建镜像，因此相当受限。让我们来看另一个：fabric8io Docker 插件。

## fabric8io Maven Docker 插件

它的配置不同。fabric8 插件使用一个配置部分来维护所有镜像。它为每个部分都有配置项，这些配置项分为 build 和 run 部分，分别用于构建指令和运行时配置。部分功能包括拉取镜像时的进度条、动态灵活的端口映射和变量分配，以及通过插件配置或 Dockerfile 构建镜像（与 Spotify 插件相同）。它还在集成测试期间记录容器的标准输出，并在容器启动后完全支持等待时间、特定 URL 或日志输出。

就配置而言，Fabric8.io Docker Maven 插件非常强大且灵活。要设置镜像配置，你需要在 <configuration><images> 元素中创建 <image> 元素，如以下示例所示：

```xml
<configuration>
  <images>
    <image>
      <alias>myApplication</alias>
      <name>me/my-application:${project.version}</name>
      <build>
<from>java:8</from>
<assembly>
    <descriptor>artifact</descriptor>
</assembly>
<cmd>
    <shell>java -jar maven/myApplication.jar</shell>
</cmd>
</build>
<run>
    <ports>
        <port>8080:8080</port>
    </ports>
</run>
</image>
</images>
</configuration>
```

![](img/0c87321919fa245e22f056c67ec70003_225_0.png)

你可以为每个要创建或使用的镜像拥有多个 <image> 元素。

镜像构建过程在 <build> 元素内配置。Fabric8.io 插件对构建配置采用两种方法；你可以使用现有的 Dockerfile，也可以使用内联配置（如前例所示）。如果你想使用现有的 Dockerfile，你需要提供额外的 dockerFileDir 和 dockerFile 元素。前者将指定包含 Dockerfile 的目录，后者将指向该目录中的特定 Dockerfile。作为 dockerFileDir 元素指定的目录中的所有文件也将被包含在镜像构建上下文中。

另一方面，内联配置将描述 pom.xml 本身中的镜像构建过程。你需要提供诸如 <from> 之类的元素，其中将包含基础镜像（相当于 Dockerfile 中的 FROM 指令）。assembly 描述符包含将进入镜像的所有文件，而 <cmd> 包含在创建容器时运行的命令。正如我们在第 6 章《Creating Images》中讨论的那样，此设置类似于标准的 Dockerfile，当然你完全支持所有 Dockerfile 标准功能。你可以暴露端口（与我们在第 6 章《Creating Images》中使用 EXPOSE Dockerfile 指令所做的方式相同）。

要暴露端口，你需要在 <ports> 元素中定义 <port> 子元素。在以下示例中，我们暴露端口号 8080，以便在运行容器时稍后将其映射到主机的端口：

```xml
<ports>
  <port>8080</port>
</ports>
```

要定义卷，需要类似的构造，并且 <volumes> 元素内部包含的每个 <volume> 元素将代表一个卷。与 VOLUME 指令完全相同，它会创建具有指定名称的挂载点，并将其标记为保存来自本地主机或其他容器的外部挂载卷。

```xml
<volumes>
  <volume>/path/to/volume</volume>
</volumes>
```

要定义镜像的入口点，你需要（正如你可能已经猜到的）一个 <entrypoint> 元素，如以下示例所示：

```xml
<entryPoint>
  <exec>
    <arg>java</arg>
    <arg>-jar</arg>
    <arg>/maven/myApplication.jar</arg>
  </exec>
</entryPoint>
```

<entrypoint> 指定了一个命令，该命令在容器启动时始终会被执行，与普通和传统的 Dockerfile 中的 ENTRYPOINT 指令方式相同。

构建配置中的 <assembly> 元素指定构建产物以及应用程序可能需要的其他文件将如何放入生成的 Docker 镜像中。有几个预定义的 assembly 描述符，例如 artifact-with-dependencies（将项目的构件及其所有依赖项附加到镜像中）、artifact（仅将项目的构件放入镜像中）、project（将整个 Maven 项目附加到镜像中）以及 rootWar（将构件作为 ROOT.war 复制到暴露的目录中）。

![](img/0c87321919fa245e22f056c67ec70003_227_0.png)

来自 assembly 描述符的所有文件都会自动进入镜像。你可以通过查看 target/docker/yourProject/build/maven 目录的内容来了解镜像中包含的内容。

正如你所见，<build> 元素中的配置几乎完全匹配 Dockerfile 的结构和内容。当你使用 Maven 执行 docker:build 命令时，它将被考虑在内：

```
mvn -PmyProfile docker:build
```

<image> 的 <run> 元素包含运行时设置；当你使用以下命令通过 Maven 创建和运行 Docker 容器时，它将被使用：

```
mvn -PmyProfile docker:build docker:start
```

在运行时配置中，你可以按以下示例定义端口映射：

```
<ports>
  <port>8080</port>
</ports>
```

在前一种情况下，容器端口 8080 将映射到 Docker 主机端口 8080。这与我们在第 4 章《Networking and Persistent Storage》中所做的相同，我们使用 docker run 命令的 -p 或 -P 选项映射了暴露的端口：

```
docker run --name nginx -d -P nginx
```

除了映射端口之外，你还可以映射卷。这再次类似于我们之前使用 docker run 命令的 -v 选项所做的映射：

```
docker run -it -v data:/data ubuntu
```

正如我们在第 4 章《Networking and Persistent Storage》中看到的那样，容器在启动时可以从各种来源挂载卷。它可以是主机系统的目录，也可以是导出一个或多个目录的另一个容器。挂载配置在 <run> 配置的 <volumes> 部分中指定。

```
<volumes>
  <bind>
    <volume>/logs</volume>
    <volume>/opt/hostDirectory:/opt/containerDirectory</volume>
  </bind>
</volumes>
```

要从另一个镜像导入卷（如我们之前使用 --volumes-from 所做的那样），你可以在卷配置中使用 <from> 和 <image> 元素。要提供镜像名称或别名，请在 <image> 元素中指定你想要使用的卷，如以下示例所示：

```
<volumes>
  <from>
    <image>test/myApplication2</image>
  </from>
</volumes>
```

fabric8.io Maven 插件是一个完整的解决方案，包含所有 Docker 相关的构建和运行时配置。你可以链接镜像、提供运行时网络设置并定义运行时重启策略（我们在第 7 章《Running Containers》中详细讨论了重启策略）。它还有一套完整的 Maven 相关目标，例如用于构建镜像的 docker:build、用于管理容器生命周期的 docker:start 或 docker:run 和 docker:stop、用于将容器上传到注册表的 docker:push，以及用于输出容器日志的 docker:logs。你甚至可以使用 maven:source 目标将生成的 Docker 镜像包含到 Maven 构件中以供进一步处理。

要执行整个操作链，即创建镜像、启动容器、执行单元测试、停止容器并进行清理，请执行 install 命令：

```
mvn -PmyProfile install
```

在使用 Maven 和 Docker 时，fabric8.io 插件提供了很多功能。你始终可以参考插件网站 https://dmp.fabric8.io/ 上的最新文档。如果需要从 Maven 构建中构建或运行 Docker 容器，它是首选。

现在我们知道如何从自己的构建过程中创建和运行 Docker 容器。现在让我们专注于将实际应用程序打包到容器中。我们将介绍示例设置，例如 Java 应用程序和 Angular.js 前端应用程序。让我们从 Java 开始；我们将对 Spring Boot 应用程序进行容器化。我们将使用我们已经了解的 fabric8 Docker Maven 插件。
