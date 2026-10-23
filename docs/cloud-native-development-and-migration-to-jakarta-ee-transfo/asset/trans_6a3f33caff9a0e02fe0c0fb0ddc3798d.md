

## 使用跨平台开源编辑器

此转换选项可在所有主要操作系统上运行。我们使用的是开源编辑器 Sublime Text，可从 `https://www.sublimetext.com/` 下载。

编辑器具有 **Find in Files** 选项，可递归搜索目录树中指定文件组中的特定文本。

启动编辑器，从 **Find** 下拉菜单中选择 **Find in files…**，然后执行以下步骤或输入 *Figure 3.1* 中显示的信息：

1. 在 **Find** 文本字段中输入 `javax.activation`。
2. 按下位于 Find 和 Replace 之间的按钮 …。
3. 选择 **Add Folder**。
4. 从选择对话框中选择您的 cargo tracker 根目录。
5. 再次按下按钮 …。
6. 选择 **Add Include Filter**。
7. 文本 `*.txt` 将插入到 **Where** 文本字段中。
8. 将 `*.txt` 改为 `*.java`。
9. 在 **Replace** 文本字段中，输入 `jakarta.activation`。

![](img/8948f94d0abcdb1ae94d6778da518d16_54_0.png)

如果现在按下 **Find** 按钮，编辑器将打开一个文档，列出所有 `javax.activation` 的出现位置。它在 9 个不同文件中出现了 10 处。

再次打开 **Find in Files** 对话框并按下 **Replace** 按钮，这次会先询问您是否要在 9 个文件中替换这 10 处出现。按下 **Yes** 将应用更改。

虽然此更改很容易，但它要求您为 *Table 3.1* 中的每一行输入值，并且对您想要迁移的每个项目都要这样做。如果您运行的是 Linux 或 macOS，下一个选项会更吸引您。

请注意，此迁移选项可从您喜爱的 IDE 内使用。

## Sed

Sed 是 Linux 和 macOS 操作系统上的标准工具。此流编辑器在 Windows 平台上不可用。如果您使用的是 Windows 操作系统，建议您使用其他两种转换选项之一。

以下代码是用于自动替换 Java EE 命名空间使用为 Jakarta EE 命名空间的小型 Bash 脚本的源代码。它由一个包含旧命名空间和新命名空间以逗号分隔列表的文本文件驱动。然后它告诉 sed 从当前目录开始递归遍历目录树，并在每个 Java 扩展名的文件上执行替换：

```
#!/bin/bash
input="./jee-jakartaee.txt"
while IFS= read -r line
do
  IFS=',' read -r -a array <<< "$line"
  sed -i -e "s/${array[0]}/${array[1]}/g" $(find ./ -name *.java)
done < "$input"
```

需要牢记两点。首先，最好将此脚本放在 cargotracker 目录的根目录中。其次，jee-jakartaee.txt 输入文件需要在同一目录中。两者均可从本书附带的 GitHub 项目下载——更准确地说，在这里：https://github.com/PacktPublishing/Cloud-Native-Development-and-Migration-to-Jakarta-EE。

## 使用 IDE 的专用插件

到目前为止，您已经看到了迁移源代码的两种选项。我们现在将展示第三种选项——可从 IDE 获得的选项。在此情况下，我们谈论的是 IntelliJ Ultimate Edition。重构是现代 IDE 中的常见功能，IntelliJ 也不例外。事实上，此迁移选项让您可以非常具体地触发想要转换的内容以及不转换的内容。更方便的是，它预定义了 Java EE 到 Jakarta EE 的迁移。

您需要在 IntelliJ 中导入并打开项目。然后，执行以下步骤：

1. 选择 Refactor 菜单选项。
2. 从下拉菜单中选择 Migrate Packages and Classes。
3. 选择 Java EE to Jakarta EE。

会出现以下对话框：

4. 选择 Whole project 选项。
5. 按下 Run 按钮。

IntelliJ 将按包名分组显示所有出现的位置。您可以通过单击对话框中显示的树来检查更改。

*Figure 3.2 – Migration dialog in IntelliJ*

*Figure 3.3 – Overview of files to migrate*

6. 最后，按下 Do Refactor 以应用更改。

完成后，编辑器将报告已更改 308 处出现。

虽然 IntelliJ Ultimate 在更改导入的命名空间方面做得很好，但它无法识别 Strings 和 XML 文件中内部的 javax 用法。为修复此缺陷，我们必须手动修复一些文件或使用前面的一种方法。

RejectedRegistrationAttemptsConsumer、MisdirectedCargoConsumer、HandlingEventRegistrationAttemptConsumer、DeliveredCargoConsumer 和 CargoHandledConsumer 都使用了一个在属性值中使用名称 javax 的注解。此注解如下所示：

```
@ActivationConfigProperty(propertyName = "destinationType", propertyValue = "javax.jms.Queue")
```

由于 propertyValue 中存在 javax，此激活配置将无法工作。为修复此问题，将注解更改为以下示例，该示例使用 jakarta：

```
@ActivationConfigProperty(propertyName = "destinationType", propertyValue = "jakarta.jms.Queue")
```

我们还需要在 web.xml 中进行更改，因为 IntelliJ 未对该文件进行任何更改。在 web.xml 文件中，javax 名称用于 javax.faces 上下文参数。在 web.xml 文件的顶部部分，您可以看到正在使用 javax 命名空间。它如下所示：

```xml
<context-param>
    <param-name>javax.faces.PROJECT_STAGE</param-name>
    <param-value>Development</param-value>
</context-param>
<context-param>
    <param-name>javax.faces.CLIENT_WINDOW_MODE</param-name>
    <param-value>url</param-value>
</context-param>
```

这些上下文参数也需要更改为新的 jakarta 命名空间。为此，您需要将类路径中的 javax 部分替换为 jakarta。结果如下所示：

```xml
<context-param>
    <param-name>jakarta.faces.PROJECT_STAGE</param-name>
    <param-value>Development</param-value>
</context-param>
<context-param>
    <param-name>jakarta.faces.CLIENT_WINDOW_MODE</param-name>
    <param-value>url</param-value>
</context-param>
```

完成这些更改后，`javax` 名称将在整个项目中被替换为 `jakarta`。最后要做的事情是在再次运行项目之前升级 POM 文件和依赖项。

## 升级您的 pom.xml 文件

我们还需要对代码库应用一个更改。我们需要告诉它现在正在使用 Jakarta EE 9 API。这是通过更新 `pom.xml` 文件中的依赖项来完成的。

打开 `pom.xml` 文件并找到依赖项部分。它应如下所示：

```xml
<dependency>
    <groupId>javax</groupId>
    <artifactId>javaee-api</artifactId>
    <version>${javaee_api.version}</version>
    <scope>provided</scope>
</dependency>
```

现在，将其更改为以下内容：

```xml
<dependency>
    <groupId>jakarta.platform</groupId>
    <artifactId>jakarta.jakartaee-api</artifactId>
    <version>9.0.0</version>
    <scope>provided</scope>
</dependency>
```

您现在已告诉 Maven 它应使用新的 Jakarta EE 9 依赖项。

现在，继续通过调用 `mvn clean install` 重新构建应用程序。

## 迁移依赖项

随着从 **Java EE** 到 **Jakarta EE** 命名空间的迁移，我们需要更新项目使用的三个依赖项。在本节中，我们将了解如何升级项目的 Java 版本、**Payara** 应用服务器以及 **PrimeFaces** 依赖项。

### 升级项目

要更改 Maven 使用的 Java 版本，我们需要在 `pom.xml` 文件中进行两次更改。在项目目录根目录中的 `pom.xml` 文件中，您可以找到 cargo tracker 应用程序的源和目标 Maven 使用。

目前，项目使用 Java 7 版本，并且在 pom.xml 文件中，您可以找到以下两行来确认这一点：

```
<maven.compiler.source>1.7</maven.compiler.source>
<maven.compiler.target>1.7</maven.compiler.target>
```

我们将 cargo tracker 应用程序升级到当前可用的最新长期支持 LTS 版本的 Java，即 17。要升级项目，请将 1.7 替换为 17，使其如下所示：

```
<maven.compiler.source>17</maven.compiler.source>
<maven.compiler.target>17</maven.compiler.target>
```

重要的是，您现在还应更改在第 2 章中设置的 JAVA_HOME 系统变量。在继续之前，下载 Java 17 SDK 并将其设置为 JAVA_HOME 系统变量。如果不这样做，项目将无法编译。

接下来是升级 Payara 应用服务器到最新版本。升级 Payara 需要两个步骤。我们需要将 Cargo 插件升级到支持 Payara 版本 6 的版本，并需要将 Payara 从 4 升级到 6。

要升级 Cargo，我们需要将版本号从 1.8.1 更改为 1.10.5。为此，请更改以下行：

```
<cargo.version>1.8.1</cargo.version>
```

结果应如下所示：

```
<cargo.version>1.10.5</cargo.version>
```

### 升级 Payara 应用服务器

我们已在 pom.xml 文件底部更改了 cargo 插件的版本。下一步是更改 Payara 容器 ID 和下载 URI。我们正从 Payara 4 迁移到 6；为此，请更改以下行：

```
<cargo.payara.container_id>glassfish4x</cargo.payara.container_id>
<cargo.payara.download_uri>https://repo1.maven.org/maven2/fish/payara/distributions/payara/6.2023.2/payara-6.2023.2.zip</cargo.payara.download_uri>
```

将其更改为如下示例：

```
<cargo.payara.container_id>glassfish6x</cargo.payara.container_id>
<cargo.payara.download_uri>https://repo1.maven.org/maven2/fish/payara/distributions/payara/6.2023.2/payara-6.2023.2.zip</cargo.payara.download_uri>
```

