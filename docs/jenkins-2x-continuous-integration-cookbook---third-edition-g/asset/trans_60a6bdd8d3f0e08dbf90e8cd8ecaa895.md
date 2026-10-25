

## 如何操作...

+   1. 进入 Jenkins 仪表盘 | 项目 | 配置 | 构建 | 添加构建步骤 | 从另一个项目复制构件。
+   2. 输入要复制构件的项目名称。
+   3. 对于“Which build”字段，从可用列表中选择合适的选项：
+   4. 提供要复制的构件名称：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_385_0.png)

![](img/2a4d2e09d9af2c678ea8f43ea344059d_386_0.png)

5. 点击保存。

## 还有更多...

+   - 你还可以指定可以复制构件的项目。
+   - 例如，在 `PetClinic-Package` 项目中你可以创建一个 WAR 文件以部署到应用服务器。
+   - 在 `PetClinic-Package` 的 General 部分，勾选 Permission to Copy Artifact 并提供可以复制构件的项目名称：

+   - 点击保存。

## 将 Jenkins 与 Artifactory 集成

Artifactory 是一个仓库管理器，允许你存储和检索构件，例如依赖项或包文件。它就像组织中的本地仓库。

## 准备工作

在本教程中，使用 https://bintray.com/jfrog/artifactory/jfrog-artifactory-oss-zip/4.15.0 作为 Artifactory。

要启动 Artifactory，进入 artifactory-oss-4.15.0 | bin 并执行 artifactory.bat。如果操作系统基于 Linux，则使用 .sh 脚本启动 Artifactory。

在浏览器中打开并访问 IP_ADDRESS:8081 以在浏览器中访问 Artifactory：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_390_0.png)

创建本地仓库以存储 Jenkins 项目创建的包文件：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_390_1.png)

![](img/2a4d2e09d9af2c678ea8f43ea344059d_393_0.png)

创建一个可从 Jenkins 使用以访问 Artifactory 的用户：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_393_1.png)

点击新建：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_394_0.png)

提供用户详细信息并保存：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_394_1.png)

验证用户列表：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_395_0.png)

为新创建的用户提供对仓库的权限：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_395_1.png)

选择 Repositories 并点击保存并完成：

检查 Artifactory 中的权限管理部分以查看最近的更改：

编辑权限并分配用户：

点击保存并完成：

检查 Artifactory 中的权限管理部分以查看最近的更改：

[368]

![](img/2a4d2e09d9af2c678ea8f43ea344059d_398_0.png)

现在，你已准备好将 Artifactory 与 Jenkins 集成。

在 Jenkins 中安装 Artifactory 插件：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_398_1.png)

插件安装成功后，你可以在 Jenkins 中配置 Artifactory 相关设置：

## 安装插件/升级

准备工作

+   - 检查互联网连接
+   - 检查更新中心连接
+   - 成功

+   - Config File Provider Plugin — 成功
+   - Ivy Plugin — 成功
+   - Artifactory Plugin — 成功

+   - [返回顶部页面](#)（你现在就可以开始使用已安装的插件）
+   - 安装完成后且没有作业运行时重启 Jenkins

## 在 Jenkins 中配置 Artifactory：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_399_0.png)

当你使用 Maven 作为构建工具时，它会创建 `.m2` 目录。这会在从互联网上可用的 Maven 仓库下载依赖项时，将所有依赖项保存在仓库目录中。

为验证目的，将 `.m2` 目录中的目录名称更改为 `repository1`：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_399_1.png)

Jenkins 构建执行开始的瞬间，它将从 Artifactory 获取依赖项，并在 `.m2` 中创建仓库目录。

4. 从添加后构建操作中选择部署构件到 Artifactory：

5. 点击刷新并选择之前创建的仓库：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_402_0.png)

+   6. 点击保存。

+   7. 点击立即构建并在控制台输出中验证日志。Jar 文件从本地仓库或 Artifactory 解析：

```
```text
[INFO] Scanning for projects...
[INFO] Initializing Artifactory Build-Info Recording
[HUDSON] Collecting dependencies info
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building petclinic 4.2.5-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] Downloading: http://localhost:8081/artifactory/libs-release/com/itextpdf/itextpdf/5.4.5/itextpdf-5.4.5.pom
log4j:WARN No appenders could be found for logger
(org.apache.maven.wagon.providers.http.httpclient.client.protocol.RequestAddCookies).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
[INFO] Downloaded: http://localhost:8081/artifactory/libs-release/com/itextpdf/itextpdf/5.4.5/itextpdf-5.4.5.pom (9 KB at 3.7 KB/sec)
[INFO] Downloading: http://localhost:8081/artifactory/libs-release/com/itextpdf/itext-parent/1.0.0/itext-parent-1.0.0.pom
[INFO] Downloaded: http://localhost:8081/artifactory/libs-release/com/itextpdf/itext-parent/1.0.0/itext-parent-1.0.0.pom (4 KB at 9.3 KB/sec)
[INFO] Downloading: http://localhost:8081/artifactory/libs-release/com/itextpdf/itextpdf/5.4.5/itextpdf-5.4.5.jar
[INFO] Downloaded: http://localhost:8081/artifactory/libs-release/com/itextpdf/itextpdf/5.4.5/itextpdf-5.4.5.jar (2054 KB at 137.0 KB/sec)
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ spring-petclinic ---
[INFO] Downloading: http://localhost:8081/artifactory/libs-release/org/apache/maven/maven-plugin-api/2.0.6/maven-plugin-api-2.0.6.pom
[INFO] Downloaded: http://localhost:8081/artifactory/libs-release/org/apache/maven/maven-plugin-api/2.0.6/maven-plugin-api-2.0.6.pom (2 KB at 0.4 KB/sec)
[INFO] Downloading: http://localhost:8081/artifactory/libs-release/org/apache/maven/maven/2.0.6/maven-2.0.6.pom
```
```

## 8. 包创建后，它也会存储在 Artifactory 中：

```
Finished Calculation of disk usage of workspace in 2 second
[INFO] Artifactory Build Info Recorder: Saving Build Info to 'F:\\DevOps2016\\#JenkinsEssentials\\FirstDraft\\jenkinsHome\\workspace\\PetClinic-Package\\target\\build-info.json'
[INFO] Deploying artifact: http://localhost:8081/artifactory/jenkins-snapshots/org/springframework/samples/spring-petclinic/4.2.5-SNAPSHOT/spring-petclinic-4.2.5-SNAPSHOT.war
log4j:WARN No appenders could be found for logger (org.apache.http.client.protocol.RequestAddCookies).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
[INFO] Artifactory Build Info Recorder: Deploying build info ...
[INFO] Deploying build descriptor to: http://localhost:8081/artifactory/api/build
[INFO] Build successfully deployed. Browse it in Artifactory under
http://localhost:8081/artifactory/webapp/builds/PetClinic-Package/14
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 03:53 min
[INFO] Finished at: 2017-08-14T17:03:32+05:30
[INFO] Final Memory: 35M/245M
[INFO] ------------------------------------------------------------------------
Waiting for Jenkins to finish collecting data
[JENKINS] Archiving F:\\DevOps2016\\#JenkinsEssentials\\FirstDraft\\jenkinsHome\\workspace\\PetClinic-Package\\pom.xml to org.springframework.samples/spring-petclinic/4.2.5-SNAPSHOT/spring-petclinic-4.2.5-SNAPSHOT.pom
[JENKINS] Archiving F:\\DevOps2016\\#JenkinsEssentials\\FirstDraft\\jenkinsHome\\workspace\\PetClinic-Package\\target\\petclinic.war to org.springframework.samples/spring-petclinic/4.2.5-SNAPSHOT/spring-petclinic-4.2.5-SNAPSHOT.war
channel stopped
Archiving artifacts
Started calculate disk usage of build
Finished Calculation of disk usage of build in 0 seconds
Started calculate disk usage of workspace
Finished Calculation of disk usage of workspace in 2 second
Finished: SUCCESS
```

## 9. 进入 Artifactory 并验证包：

