

![](img/2a4d2e09d9af2c678ea8f43ea344059d_298_0.png)

- 4. 任何包含 HTML、CSS 和 JavaScript 文件的项目都可以使用。

### 如何操作...

- 1. 进入 Build 部分并选择 Execute SonarQube Scanner：

  ![](img/2a4d2e09d9af2c678ea8f43ea344059d_299_0.png)

- 2. 你可以提供 `sonar-project.properties` 的位置，或直接提供静态代码分析的详细信息。

```properties
# Required metadata
sonar.projectKey=SonarHTMLCSSJS
sonar.projectName=Simple HTML CSS JS project analyzed with the SonarQube
sonar.projectVersion=1.0
# Comma-separated paths to directories with sources (required)
sonar.sources=.
# Encoding of the source files
sonar.sourceEncoding=UTF-8
```

- 3. `sonar.sources` 是静态代码分析的主要属性。通过此属性，你可以告知 SonarQube 需要分析哪个目录：

  ![](img/2a4d2e09d9af2c678ea8f43ea344059d_300_0.png)

- 4. 点击 Save。
- 5. 进入 Jenkins Project 并点击 Build now。
- 6. 进入 Console output 查看日志。
- 7. 仅观察以下代码中给出的日志。
- 8. 你会注意到，根据可用的文件，SonarQube 会自动选择不同的质量配置文件并分析代码：
- 9. Started by user admin.
- 10. [EnvInject] - Loading node environment variables.
- 11. Building in workspace:
    F:\1.DevOps2016\#JenkinsEssentials\FirstDraft\jenkinsHome\workspace\SonarHTMLCSSJS
- 12. Cloning the remote Git repository:
    Cloning repository https://github.com/claviska/shoelace-css.git
    >git.exe init
    F:\1.DevOps2016\#JenkinsEssentials\FirstDraft\jenkinsHome\workspace\SonarHTMLCSSJS
    # timeout=10
- 13. Fetching upstream changes from https://github.com/claviska/shoelace-css.git:
    > git.exe --version # timeout=10
    >git.exe fetch --tags --progress https://github.com/claviska/shoelace-css.git
    +refs/heads/*:refs/remotes/origin/*
    >git.exe config remote.origin.url https://github.com/claviska/shoelace-css.git # timeout=10
    >git.exe config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
    >git.exe config remote.origin.url https://github.com/claviska/shoelace-css.git # timeout=10
- 14. Fetching upstream changes from https://github.com/claviska/shoelace-css.git:
    >git.exe fetch --tags --progress https://github.com/claviska/shoelace-css.git
    +refs/heads/*:refs/remotes/origin/*
    >git.exe rev-parse "refs/remotes/origin/master{commit}" # timeout=10
    >git.exe rev-parse "refs/remotes/origin/origin/master{commit}" # timeout=10
- 15. Checking out revision 39b44e9519e5050675e01527c421c5e0f3a046b7 (refs/remotes/origin/master):

```
>git.exe config core.sparsecheckout # timeout=10
>git.exe checkout -f 39b44e9519e5050675e01527c421c5e0f3a046b7
```

- 16. First time build. Skipping changelog:

```
[SonarHTMLCSSJS] $
F:\1.DevOps2016\#JenkinsEssentials\FirstDraft\jenkinsHome\tools\hudson.plugins.soscanner.bat -e -Dsonar.host.url=http://localhost:9000/ ******* "-Dsonar.projectName=Simple HTML CSS JS project analyzed with the SonarQube" -Dsonar.projectVersion=1.0 -Dsonar.sourceEncoding=UTF-8 -Dsonar.projectKey=SonarHTMLCSSJS -Dsonar.sources=. -Dsonar.projectBaseDir=F:\1.DevOps2016\#JenkinsEssentials\FirstDraft\jenkinsHome\workspace\SonarHTMLCSSJS

INFO: Option -e/--errors is no longer supported and will be ignored
INFO: Scanner configuration file:
F:\1.DevOps2016\#JenkinsEssentials\FirstDraft\jenkinsHome\tools\hudson.plugins.soscanner.properties
INFO: Project root configuration file: NONE
INFO: SonarQube Scanner 3.0.3.778
INFO: Java 1.8.0_111 Oracle Corporation (64-bit)
INFO: Windows 10 10.0 amd64
INFO: User cache: C:\WINDOWS\system32\config\systemprofile\.sonar\cache
INFO: Load global settings
INFO: Load global settings (done) | time=557ms
INFO: User cache: C:\WINDOWS\system32\config\systemprofile\.sonar\cache
INFO: Load plugins index
INFO: Load plugins index (done) | time=113ms
INFO: Download sonar-csharp-plugin-5.7.0.612.jar
INFO: Download sonar-python-plugin-1.7.0.1195.jar
INFO: Download sonar-css-plugin-3.1.jar
INFO: Download sonar-java-plugin-4.12.0.11033.jar
INFO: Download sonar-scm-git-plugin-1.2.jar
INFO: Download sonar-android-plugin-1.1.jar
INFO: Download sonar-php-plugin-2.9.2.1744.jar
INFO: Download sonar-scm-svn-plugin-1.4.0.522.jar
INFO: SonarQube server 6.3.1
INFO: Default locale: "en_IN", source code encoding: "UTF-8"
INFO: Process project properties
INFO: Load project repositories
INFO: Load project repositories (done) | time=140ms
INFO: Load quality profiles
INFO: Load quality profiles (done) | time=437ms
INFO: Load active rules
INFO: Load active rules (done) | time=6984ms
INFO: Load metrics repository
INFO: Load metrics repository (done) | time=752ms
INFO: Publish mode
INFO: Project key: SonarHTMLCSSJS
INFO: --------- Scan Simple HTML CSS JS project analyzed with the SonarQube
INFO: Load server rules
INFO: Load server rules (done) | time=1241ms
INFO: Initializer GenericCoverageSensor
INFO: Initializer GenericCoverageSensor (done) | time=0ms
INFO: Base dir:
F:\1.DevOps2016\#JenkinsEssentials\FirstDraft\jenkinsHome\workspace\SonarHTMLCSSJS
INFO: Working dir:
F:\1.DevOps2016\#JenkinsEssentials\FirstDraft\jenkinsHome\workspace\SonarHTMLCSSJS
INFO: Source paths: .
INFO: Source encoding: UTF-8, default locale: en_IN
INFO: Index files
INFO: 30 files indexed
INFO: Quality profile for css: SonarQube Way
INFO: Quality profile for js: Sonar way
INFO: Quality profile for web: Sonar way
INFO: Sensor NoSonar Sensor [php]
INFO: Sensor NoSonar Sensor [php] (done) | time=0ms
INFO: Sensor Coverage Report Import [csharp]
INFO: Sensor Coverage Report Import [csharp] (done) | time=0ms
```

![](img/2a4d2e09d9af2c678ea8f43ea344059d_303_0.png)

- 19. 静态代码分析中使用了默认的质量配置文件和质量门禁。

- 20. 可用 Bugs & Vulnerabilities 和 Code Smells 相关详情。点击每个项目可根据文件获取更多详情。

- 21. 此处 Quality Gate 已失败，未满足条件：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_303_1.png)

- 22. 进入 Code 选项卡以获取文件和目录的 Bugs & Vulnerabilities 和 Code Smells：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_304_0.png)

- 23. 点击 Activity 以获取项目分析历史：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_305_0.png)

- 24. 进入 Measures | Maintainability 以获取评级和技术债务详情：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_305_1.png)

- 25. 进入 Measures | Size 以获取代码异味大小的图表：

- 26. 进入同一层级的不同子部分并深入观察详情以获得更多理解。

![](img/2a4d2e09d9af2c678ea8f43ea344059d_308_0.png)

- 2. 有些情况下某些问题是误报。在这种情况下，可从 SonarQube 仪表板将其配置为已修复。

## 使用 SonarQube 验证 Java 代码

现在让我们创建一个新的 Quality Profile 并分配项目，以便每次执行静态代码分析时不使用默认配置文件而使用自定义配置文件。

### 准备工作

- 1. 进入 Quality Profiles，并在 Java 部分复制默认配置文件：

  ![](img/2a4d2e09d9af2c678ea8f43ea344059d_310_0.png)

- 2. 为其指定特定名称并点击 Copy：

  ![](img/2a4d2e09d9af2c678ea8f43ea344059d_310_1.png)

- 3. 我们也可以通过点击 Change Projects 来为特定 Quality Profile 指定项目：

## Build

## Execute SonarQube Scanner

Task to run

JDK

JDK to be used for this SonarQube analysis

Path to project properties

Analysis properties

```
# Language
sonar.language=java

# Encoding of the source files
sonar.sourceEncoding=UTF-8
```

Additional arguments

JVM Options

Add build step

## Post-build Actions

Save

Apply

- 6. 点击 Save 然后 Build now。

- 7. 进入 SonarQube 仪表板并检查 Quality Gate 和 Quality Profiles：

