

## 设置开发环境

在开始之前，重要的是要了解 Alfresco Maven SDK 与 Alfresco Community 4.2 及以上版本、Alfresco Enterprise 4.1.2 及以上版本兼容。有趣的是，使用 Alfresco Maven SDK 时，你无需在机器上手动安装 Alfresco。Alfresco Maven SDK 将使你的工作更加轻松，它会根据项目对象模型（POM）XML 中为版本和 groupId 提供的条目，自动在你的机器上下载 Alfresco。

现在，在开始使用 Alfresco Maven SDK 创建项目之前，让我们首先确保开发机器上已经安装了所需的前置条件：

- 你需要在机器上安装并正确配置 JDK 1.7。**Java 开发工具包（JDK）** 基本上是一个使用 Java 编程语言构建应用程序的开发环境。你可以从 `http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html` 下载。下载并安装后，请确保设置 JAVA_HOME 环境变量以及 **Path** 变量。

- 你需要安装并正确配置 Apache Maven 3.0.3 或更高版本。你可以从 `http://maven.apache.org/download.cgi` 下载。你还会找到基于你所用操作系统的安装说明。成功下载并安装后，请确保环境变量 M2E_HOME 已正确设置，并且相应修改了 **Path** 变量。

现在，让我们通过运行 `mvn -version` 命令来检查 Maven 是否指向正确的 JDK 安装，并确保你已安装正确版本的 Maven。在 Windows 机器上，从命令提示符运行此命令将显示有关 Maven 版本、Maven 主目录、Maven 使用的 Java 版本以及 Java 主目录的信息：

```
C:\Users\ramesh.chauhan>mvn -version
Apache Maven 3.0.4 (r1232337; 2012-01-17 14:14:56+0530)
Maven home: D:\apache-maven-3.0.4\bin\..
Java version: 1.7.0_51, vendor: Oracle Corporation
Java home: C:\Program Files\Java\jdk1.7.0_51\jre
Default locale: en_US, platform encoding: Cp1252
OS name: "windows 7", version: "6.1", arch: "amd64", family: "windows"
```

## 探索 Alfresco Maven 仓库

Alfresco Maven 仓库基本上是一个公共位置，所有与 Alfresco 相关的依赖 JAR 都保存在那里，你可以在项目的 pom.xml 文件中配置项目以引用仓库中的所需依赖。你可能希望探索 Alfresco Maven SDK 将使用的 Maven 仓库。访问 https://artifacts.alfresco.com/nexus/#view-repositories 并探索包含已发布和稳定工件的公共发布仓库。此外，在 https://artifacts.alfresco.com/nexus/service/local/repo_groups/public/content/archetype-catalog.xml 的 archetype-catalog 下，可以找到不同的可用默认项目模板，这些模板在 Maven 术语中称为原型（archetype）。

## 为 AMP 创建默认项目结构

为了创建 AMP 的项目结构，我们将使用 Alfresco Maven SDK 提供的项目模板。这个默认项目模板基本上被称为 AMP 原型。此项目模板有助于为你将为项目开发的 Alfresco 扩展提供标准化的开发、部署和发布方法。Alfresco AMP 原型可用于在需要时创建 repository 和 share 的 AMP。在本章中，我们将使用 AMP 原型来开发 repository web scripts。

> Alfresco Maven SDK 还提供了另一个有用的原型，称为 All-in-One 原型，它基本上是一个多模块项目，并提供了 Alfresco 平台及其组件的完整安装，无需额外下载。在探索 archetype-catalog.xml 时，你可能已经注意到有很多可用的原型。重要的原型，即 Alfresco AMP 原型和 All-in-One 原型，可以通过 archetype-catalog.xml 中具有 org.alfresco.maven.archetype group ID 来识别。

为了使用 Alfresco AMP 原型创建默认 AMP 项目，请执行以下步骤：

1. 从控制台运行以下命令。这里需要注意，如果 https 链接对你不适用，你可以在以下代码片段中尝试使用 http 代替 https：

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

2. 您将看到可用的原型及其简要说明，如下面的屏幕截图所示。系统将提示您选择一个原型。输入 1 并按回车，如下面的屏幕截图所示：

```
Choose archetype:
1: https://artifacts.alfresco.com/nexus/content/groups/public/archetype-catalog.xml -> org.alfresco.maven.archetype:alfresco-amp-archetype [Sample project with full support for lifecycle and rapid development of AMPs (Alfresco Module Packages)]
2: https://artifacts.alfresco.com/nexus/content/groups/public/archetype-catalog.xml -> org.alfresco.maven.archetype:alfresco-allinone-archetype [Sample multi-module project for All-in-One development on the Alfresco platform. Includes modules for Repository, AMP, Share, Solr, Web Quick Start and embedded Jetty run]
Choose a number or apply filter (format: [groupId:artifactId, case sensitive contains]): : 1
```

3. 在下一步中，系统将要求您选择作为项目基础的原型版本。这将列出您所选原型的可用版本。您也可以从 archetype-catalog.xml 文件中找到这些版本。对于 AMP 原型，1.1.1 是目前的最新版本。因此，输入 5 并按回车，如下面的屏幕截图所示：

```
Choose org.alfresco.maven.archetype:alfresco-amp-archetype version:
1: 1.0
2: 1.0.1
3: 1.0.2
4: 1.1.0
5: 1.1.1
Choose a number: 5: 5
```

4. 接下来，系统将提示您为 groupId 提供值，您可以将其视为 Java 术语中的包名。您可以为您的项目提供任何适合的名称。在这里，我们将提供 com.example 作为 groupId，如下面的代码所示：

```
Define value for property 'groupId': : com.example
```

7. 现在，在 Maven 进行一些处理后，它将在控制台上显示 **构建成功** 消息，并显示从原型创建的项目位置作为信息性消息，如下面的屏幕截图所示：

![](img/f02f759059b86af67e38b543d498a0d8_151_0.png)

它已为 Alfresco 项目生成所需的示例项目结构，以生成 AMP 文件。现在，您可以转到创建默认项目结构的位置并进行探索。这里项目结构创建在 `C:\Users\ramesh.chauhan\example-repo` 位置。在生成的项目结构中，您会发现的最重要文件是 `pom.xml`，每个 Maven 项目基本上都使用它来向 Maven 指示项目依赖关系。除了 `pom.xml`，生成的项目结构还包含默认配置文件、示例 Java 代码及其支持的单元测试，以及用于测试的运行本地 Alfresco 实例的配置。

## 从默认项目生成 AMP

一旦默认项目结构准备就绪，让我们尝试为其生成 AMP 文件。转到所创建项目结构的位置。要为其创建 AMP 文件，请运行 `mvn package -DskipTests=true` 命令。

在这里，我们在生成 AMP 时并不想运行测试；因此，我们明确将 `skipTests` 设置为 true。如果您也想运行测试，则只需执行 `mvn package` 命令。

