
## 构建项目的先决条件

在深入构建工件之前，让我们列出开发环境的先决条件：

- Apache Maven 版本 3+。
- Java JDK 1.8。
- `settings.xml` 文件（[https://github.com/pentaho/maven-parent-poms/blob/master/maven-support-files/settings.xml](https://github.com/pentaho/maven-parent-poms/blob/master/maven-support-files/settings.xml)）存储在你的 `/.m2` 目录中（`.m2` 文件夹由 Apache Maven 用于托管依赖项）。`settings.xml` 文件包含从 Pentaho 的 Sonatype Nexus 仓库管理器（[https://public.nexus.pentaho.org/](https://public.nexus.pentaho.org/)）获取库和依赖项所需的一切。

当然，你的开发环境中应提供 `git` 客户端以本地克隆仓库。

**git clone https://github.com/pentaho/pentaho-reporting.git**

`git` 客户端将开始下载整个仓库及其所有子项目，几分钟后，完整的源代码将可在 `pentaho-reporting` 文件夹中使用。在根文件夹的 `README.md` 文件中，你可以找到多个主题的详细描述，例如：

- 如何构建它（在不同平台上）
- 如何运行测试和集成测试
- 如何在 IntelliJ IDE 上导入项目
- 如何安装它以及在哪里找到文档

在以下章节中，你将重点关注构建和运行 nightly build 的描述，以便你能够使用最新的（可能不稳定）版本进行评估。

## 构建发行版

在 `README.md` 文件中对构建发行版有很好的描述。所有必需的配置文件都由名为 `release` 的属性存在而激活。如果你还要构建 macOS 发行版，则还必须指定 `mac.jre.path` 属性，如以下命令所示：**mvn clean install -Drelease -Dmac.jre.path=YOUR_MAC_OS_JRE_PATH** **(YOUR_MAC_OS_JRE_PATH example jre1.8.0_121.jre\Contents\Home)**

启动后，命令将开始下载大量依赖项，并会出现很长的日志信息尾部。子项目构建完成后，单元测试将被启动，同时也会执行构建最终包所需的一切。

![](img/9337ac94537bf7e092e1ea6531dce8fd_690_0.png)

> *我希望你不必在几分钟内完成构建。取决于你的网络连接，下载依赖项时的任何失败以及其他一些意外行为，你可能需要超过半小时才能完成此任务。*

执行结束时，最终工件将以 `prd-ce-<version>.zip` 的名称生成在 `assemblies/winlinux(mac)/target` 文件夹中。该 ZIP 文件是你运行 Pentaho Report Designer 所需的一切，也是基于 Pentaho Reporting 构建个人项目所需的内容。

## 安装和运行 nightly build

从这里开始，安装任务与你在第 2 章 *开始使用报表设计器* 中学到的内容没有区别：解压 ZIP 文件到最终目标位置，打开终端，进入 `report-designer` 文件夹，并启动 `report-designer` 脚本。

## Pentaho Reporting SDK 夜间构建

一旦 Pentaho Reporting 工件构建完成，所有 Pentaho Reporting 库都将在其中可用，如第 1 章 *Pentaho Reporting 简介* 中所述。在本节中，你将学习如何在 Java 项目中使用 nightly build，使用 Pentaho Reporting 的最新（可能不稳定）版本进行评估。

假设你有一个使用 Pentaho Reporting 的现有 Java 项目，让我们在以下章节中了解如何更新它以使用 nightly build。作为示例，你可以使用 GitHub 仓库 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 中提供的示例之一。

为准备项目，将其复制到工作文件夹中。作为示例，你可以使用存储在 `Chapter 03 - Getting started with Reporting SDK/pentaho-reporting-web-app` 文件夹中的项目。

## 更新 pom.xml 文件

准备现有项目的首要任务是更改 pom.xml 文件。使用编辑器打开 pom.xml，你会看到所有 Pentaho 依赖项都指向 <version>8.0-SNAPSHOT</version>。当然，如果你的项目使用不同版本，实际上没有任何变化。

对于所有 Pentaho 依赖项，将版本标签更新为 nightly build 版本。在我们的示例中，你将使用 <version>8.1-SNAPSHOT</version>。同样，如果你的项目使用不同版本，实际上没有任何变化。

保存 pom.xml 文件就是更新 Java 项目所需的一切。

## 添加 nightly build 库

如果你现在运行项目的构建，Apache Maven 客户端将尝试从 pom.xml 文件（在我们的示例中为 8.1-SNAPSHOT）请求下载所有 Pentaho 依赖项的最新版本。考虑到最新版本尚未发布到 Pentaho 的 Sonatype Nexus 仓库中，将会发生错误，你的构建将失败。

为了解决此问题，有一种变通方法，即将 Pentaho 最新依赖项添加到你的本地 Apache Maven 仓库。简而言之，当 Apache Maven 查找依赖项时，它首先在本地仓库（.m2/repository 文件夹）中查找，如果未找到，则尝试从远程仓库下载。通过将最新的 Pentaho 依赖项添加到本地 Apache Maven 仓库，你将跳过访问远程仓库的需求，从而避免失败。

> *请注意，这是一种变通方法，可让你使用最新的（可能不稳定）Pentaho Reporting nightly build。将库添加到本地 Maven 仓库不是最佳实践，应尽可能限制。*

要将库或一般工件安装到本地 Apache Maven 仓库，有一个类似以下命令的命令：

```
mvn
  install:install-file
  -Dfile=path/to/artifact/name.jar
  -DgroupId=...
  -DartifactId=...
  -Dversion=...
  -Dpackaging=jar
```

通过对每个 Pentaho 依赖项使用此命令，你可以使用 nightly build 更新本地 Apache Maven 仓库。以下是要运行的命令列表，以添加所有请求的依赖项，成功构建本书中描述的所有示例：**mvn install:install-file -Dfile=./report-designer/lib/classic-core-8.1-SNAPSHOT.jar -DgroupId=pentaho-reporting-engine -DartifactId=pentaho-reporting-engine-classic-core -Dversion=8.1-SNAPSHOT -Dpackaging=jar**

```
mvn install:install-file -Dfile=./report-designer/lib/classic-extensions-8.1-SNAPSHOT.jar -DgroupId=pentaho-reporting-engine -DartifactId=pentaho-reporting-engine-classic-extensions -Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/wizard-core-8.1-SNAPSHOT.jar -DgroupId=pentaho-reporting-engine -DartifactId=pentaho-reporting-engine-wizard-core -Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/libbase-8.1-SNAPSHOT.jar -DgroupId=pentaho-library -DartifactId=libbase -Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/libdocbundle-8.1-SNAPSHOT.jar -DgroupId=pentaho-library -DartifactId=libdocbundle -Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/libfonts-8.1-SNAPSHOT.jar -DgroupId=pentaho-library -DartifactId=libfonts -Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/libformat-8.1-SNAPSHOT.jar -DgroupId=pentaho-library -DartifactId=libformat -Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/libformula-8.1-SNAPSHOT.jar -DgroupId=pentaho-library -DartifactId=libformula -Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/libloader-8.1-SNAPSHOT.jar -DgroupId=pentaho-library -DartifactId=libloader -Dversion=8.1-SNAPSHOT -Dpackaging=jar
```

```
mvn install:install-file -Dfile=./report-designer/lib/librepository-8.1-SNAPSHOT.jar -DgroupId=pentaho-library -DartifactId=librepository -
```
