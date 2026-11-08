
```
<project ...>
 ...
 <!-- Add This here. -->
<repositories>
 <repository>
 <id>pentaho-releases</id>
 <url>http://repository.pentaho.org/artifactory/repo/</url>
</repository>
 </repositories>

 <dependencies>
...
 </dependencies>
 <build>
 ...
 </build>
</project>
```

此 repositories 部分定义了 Pentaho 仓库的 URL，所有与 Pentaho 相关的依赖项都将从此处下载。
保存 pom.xml 文件以完成任务。

## 添加 Pentaho 依赖项

一旦 Pentaho Nexus 仓库已包含在你的 Web 应用中，就该声明定义 Pentaho Report Engine 的所有组件的依赖项了。要在 Java 项目中添加 Pentaho 依赖项，请将以下 XML 代码添加到位于 pentaho-reporting-web-app 文件夹中的 pom.xml 文件：

```
<project ...>
...
  <repositories>
  ...
  </repositories>
  <dependencies>
  ...

    <!-- Add This here. -->
    <dependency>
      <groupId>pentaho-reporting-engine</groupId>
      <artifactId>pentaho-reporting-engine-classic-core</artifactId>
      <version>8.0-SNAPSHOT</version>
    </dependency>
    <dependency>
      <groupId>pentaho-reporting-engine</groupId>
      <artifactId>pentaho-reporting-engine-classic-extensions</artifactId>
      <version>8.0-SNAPSHOT</version>
    </dependency>
    <dependency>
      <groupId>pentaho-reporting-engine</groupId>
      <artifactId>pentaho-reporting-engine-wizard-core</artifactId>
      <version>8.0-SNAPSHOT</version>
    </dependency>
    <dependency>
      <groupId>pentaho-library</groupId>
      <artifactId>libbase</artifactId>
      <version>8.0-SNAPSHOT</version>
    </dependency>
    <dependency>
      <groupId>pentaho-library</groupId>
      <artifactId>libdocbundle</artifactId>
      <version>8.0-SNAPSHOT</version>
    </dependency>
    <dependency>
      <groupId>pentaho-library</groupId>
      <artifactId>libfonts</artifactId>
      <version>8.0-SNAPSHOT</version>
    </dependency>
    <dependency>
      <groupId>pentaho-library</groupId>
      <artifactId>libformat</artifactId>
      <version>8.0-SNAPSHOT</version>
    </dependency>
    <dependency>
      <groupId>pentaho-library</groupId>
      <artifactId>libformula</artifactId>
      <version>8.0-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>pentaho-library</groupId>
    <artifactId>libloader</artifactId>
    <version>8.0-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>pentaho-library</groupId>
    <artifactId>librepository</artifactId>
    <version>8.0-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>pentaho-library</groupId>
    <artifactId>libserializer</artifactId>
    <version>8.0-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>pentaho-library</groupId>
    <artifactId>libxml</artifactId>
    <version>8.0-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>com.lowagie</groupId>
    <artifactId>itext</artifactId>
    <version>2.1.7</version>
</dependency>
<dependency>
    <groupId>com.lowagie</groupId>
    <artifactId>itext-rtf</artifactId>
    <version>2.1.7</version>
</dependency>
<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.2</version>
</dependency>

</dependencies>
<build>
...
</build>
</project>
```

如果要在报表中使用图表，还应添加以下依赖项：

```
<dependency>
    <groupId>pentaho-reporting-engine</groupId>
    <artifactId>pentaho-reporting-engine-legacy-charts</artifactId>
    <version>8.0-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>org.jfree</groupId>
    <artifactId>jfreechart</artifactId>
    <version>1.0.19</version>
</dependency>
<dependency>
    <groupId>bsf</groupId>
    <artifactId>bsf</artifactId>
    <version>2.4.0</version>
</dependency>
```

![](img/9337ac94537bf7e092e1ea6531dce8fd_140_0.png)

在本书开发时，Pentaho Reporting SDK 的最新可用版本为 8.0。当然，这在未来会发生变化，因此你应相应地替换 version 标签的内容。在本书开发时，Pentaho Reporting SDK 的最新可用版本为 8.0。当然，这在未来会发生变化，因此你应相应地替换 version 标签的内容。建议检查位于 https://public.nexus.pentaho.org 的 Pentaho Nexus 服务器并搜索最新可用版本，使用搜索词 `pentaho-reporting-engine-classic-core`。

## 在 Web 应用中显示现有 Pentaho 报表
在 Web 应用中

现在 Java Web 应用已准备好管理 Pentaho 内容，在此最后任务中，你将最终学习如何修改 Java Web 应用以显示使用 Report Design Wizard 开发的 Pentaho 报表。特别是，目标是显示之前作为示例开发的 `my_first_report`。

![](img/9337ac94537bf7e092e1ea6531dce8fd_141_0.png)

_在接下来的章节中，你会看到使用 Report Design Wizard 未开发的 Pentaho 报表的管理方式大致相同。因此，本内容对你学习如何从 Java 应用管理现有 Pentaho 报表很有价值。_

像往常一样，我们将任务拆分为较小的子任务，因为这样描述、检查和控制起来会更容易。

## 包含现有 Pentaho 报表

非常 first task 是将 `my_first_report` 包含在 Java Web 应用中。记住 `my_first_report.prpt` 文件包含了渲染 Pentaho 报表所需的一切，请创建路径 `pentaho-reporting-web-app/src/main/resources/reports` 并将 `my_first_report.prpt` 文件复制到其中。

信不信由你，但这就是使报表对 Java Web 应用可用所需做的全部工作。

## 包含 SampleData 模式

如第 2 章 *开始使用 Report Designer* 中所述，`my_first_report` 报表使用 `SampleData` 模式作为数据源。因此，你需要在 Java Web 应用中包含它。将 `SampleData` 模式包含在 Java Web 应用中可以通过两个简单的任务实现：使 `SampleData` HyperSQL 数据库在项目中可用以及添加 HyperSQL 客户端作为依赖项。

要使 `SampleData` HyperSQL 数据库在 Java Web 应用中可用，请创建路径 `pentaho-reporting-web-app/resources` 并将 `sampledata` 文件夹从 `<report-designer>/resources` 复制到其中。

要将 HyperSQL 客户端添加为 Java Web 应用的依赖项，请根据以下描述更改位于 `pentaho-reporting-web-app` 文件夹中的 `pom.xml` 文件：

```
<project ...>
...
  <repositories>
  ...
  </repositories>
  <dependencies>
  ...

  <!-- Add This here. -->
  <dependency>
    <groupId>org.hsqldb</groupId>
    <artifactId>hsqldb</artifactId>
    <version>2.3.2</version>
  </dependency>

  </dependencies>
  <build>
  ...
  </build>
</project>
```

这就是将 `SampleData` 模式包含在你的 Java Web 应用中所需做的全部工作。

```java
package com.example;

import java.io.IOException;
import java.net.URL;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.pentaho.reporting.engine.classic.core.ClassicEngineBoot;
import org.pentaho.reporting.engine.classic.core.MasterReport;
import org.pentaho.reporting.engine.classic.core.modules.output.pageable.pdf
import org.pentaho.reporting.libraries.resourceloader.Resource;
import org.pentaho.reporting.libraries.resourceloader.ResourceManager;

public class PentahoServlet extends HttpServlet {

    @Override
    public void init(
        ServletConfig config)
    throws ServletException {
        super.init(config);
        ClassicEngineBoot.getInstance().start();
    }

    @Override
    public void doGet(
        HttpServletRequest request,
        HttpServletResponse response)
    throws ServletException,
    IOException {
        doPost(request, response);
    }

    @Override
    public void doPost(
        HttpServletRequest request,
        HttpServletResponse response)
    throws ServletException, IOException {
        // Prpt file.
        String reportPath = "file:" +
        this.getServletContext().getRealPath("WEB-INF/classes/reports/my_first_report.prpt");

        try {

            // Resource manager.
            ResourceManager manager = new
            ResourceManager();
            manager.registerDefaults();

            // Get report.
            Resource res = manager.createDirectly(new
            URL(reportPath), MasterReport.class);
            MasterReport report =
            (MasterReport) res.getResource();

            // Generate report in
```
