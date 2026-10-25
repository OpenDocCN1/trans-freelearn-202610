
# 另请参阅

-   *《在 Maven 中通过 Groovy 运行 Ant》*配方

# 在 Maven 中通过 Groovy 运行 Ant

Jenkins 所面向的用户具有广泛的技术背景。许多开发者在转向使用 Maven 之前就已精通 Ant 脚本编写，他们可能更乐于编写 Ant 任务，而不是编辑 pom.xml 文件。在相当一部分组织中，仍然运行着关键业务的 Ant 脚本。

在 Maven 中，你可以直接使用 AntRun 插件（http://maven.apache.org/plugins/maven-antrun-plugin/）或通过 Groovy（http://docs.codehaus.org/display/GROOVY/Using+Ant+from+Groovy）来运行 Ant 任务。AntRun 代表了一条自然的迁移路径，也是初期工作最少的路径。

对于将 Groovy 作为任务一部分的 Jenkins 管理员来说，Groovy 方法很有意义。作为一种一流的编程语言，Groovy 拥有许多 Ant 难以复制的控制结构。你可以使用 Ant-contrib 库（http://ant-contrib.sourceforge.net）来部分实现这些控制结构。然而，作为一种功能丰富的编程语言，Groovy 的表达能力要强得多。

本配方详细介绍了如何运行两个涉及 Groovy 和 Ant 的 Maven POM 文件。第一个 POM 展示了如何在 Groovy 中运行最简单的 Ant 任务，第二个 POM 则执行一个 Ant-Contrib 任务，以安全地从大量计算机复制文件。

# 准备工作

创建一个名为 `ch3.building_software/antbuilder` 的目录。

# 如何操作...

- 1. 创建一个模板文件并将其命名为 `pom_ant_simple.xml`。
- 2. 将 `groupId`、`artifactId`、`version` 和 `name` 的值改为适合你的值。
- 3. 在 `</project>` 标签之前添加以下 XML 片段：

```xml
<build>
  <plugins><plugin>
    <groupId>org.codehaus.gmaven</groupId>
    <artifactId>gmaven-plugin</artifactId>
    <version>1.3</version>
    <executions>
      <execution>
        <id>run-myGroovy-test</id>
        <goals><goal>execute</goal></goals>
        <phase>test</phase>
        <configuration>
          <source>
            def ant = new AntBuilder()
            ant.echo("\n\nTested ----> With Groovy")
          </source>
        </configuration>
      </execution>
      <execution>
        <id>run-myGroovy-verify</id>
        <goals><goal>execute</goal></goals>
        <phase>verify</phase>
        <configuration>
          <source>
            def ant = new AntBuilder()
            ant.echo("\n\nVerified at ${new Date()}")
          </source>
        </configuration>
      </execution>
    </executions>
  </plugin></plugins>
</build>
```

- 4. 运行 `mvn test -f pom_ant_simple.xml`。查看输出（注意不会出现关于空 JAR 文件的警告）：
- 5. 运行 `mvn verify -f pom_ant_simple.xml`。查看输出，它应该类似于以下截图：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_354_0.png)

- 4. 创建第二个模板文件并将其命名为 `pom_ant_contrib.xml`。
- 5. 将 `groupId`、`artifactId`、`version` 和 `name` 的值改为适合你的值。
- 6. 在 `</project>` 标签之前添加以下 XML 片段：

```xml
<build>
<plugins><plugin>
<groupId>org.codehaus.gmaven</groupId>
<artifactId>gmaven-plugin</artifactId>
<version>1.3</version>
<executions>
<id>run-myGroovy</id>
<goals><goal>execute</goal></goals>
<phase>verify</phase>
<configuration>
<source>
def ant = new AntBuilder()
host="Myhost_series"
print "user: "
user = new String(System.console().readPassword())
print "password: "
pw = new String(System.console().readPassword())

for ( i in 1..920) {
  counterStr=String.format('%02d',i)
  ant.scp(trust:'true',file:"${user}:${pw}@${host}${counterStr}:/${full_path_to_loca

localTofile:"${myfile}-${counterStr}", verbose:"true")
}
</source>
</configuration>
</execution>
</executions>
<dependencies>
<dependency>
<groupId>ant</groupId>
<artifactId>ant</artifactId>
<version>1.6.5</version>
</dependency>
<dependency>
```

```xml
<groupId>ant</groupId>
<artifactId>ant-launcher</artifactId>
<version>1.6.5</version>
</dependency>
<dependency>
<groupId>ant</groupId>
<artifactId>ant-jsch</artifactId>
<version>1.6.5</version>
</dependency>
<dependency>
<groupId>com.jcraft</groupId>
<artifactId>jsch</artifactId>
<version>0.1.42</version>
</dependency>
</dependencies>
</plugin></plugins>
</build>
```

这只是示例代码，除非你已将其设置为指向真实服务器上的真实文件：

```text
mvn verify -f pom_ant_simple.xml will fail
```

# 工作原理...

Groovy 无需额外依赖即可运行基本的 Ant 任务。系统会创建一个 `AntBuilder` 实例（http://groovy.codehaus.org/Using+Ant+Libraries+with+AntBuilder），然后调用 Ant 的 echo 任务。在底层，Groovy 会调用 Ant 用于执行 `echo` 命令的 Java 类。在 `echo` 命令中，通过直接创建一个匿名对象来打印日期：

```
ant.echo("\n\nVerified at ${new Date()}").
```

你将 `pom.xml` 文件配置为在两个阶段触发 Groovy 脚本：test 阶段，以及随后的 verify 阶段。test 阶段发生在生成 JAR 文件之前，因此避免了创建空 JAR 文件的警告。顾名思义，该阶段适用于打包前的测试。

第二个示例脚本凸显了 Groovy 与 Ant 结合的优势。SCP 任务（http://ant.apache.org/manual/Tasks/scp.html）在多台服务器上被多次运行。脚本首先要求输入用户名和密码，避免将其存储在文件系统或版本控制系统中。Groovy 脚本需要你注入 `host`、`full_path_to_location` 和 `myfile` 变量。

请注意 Ant SCP 任务与其在 `pom_ant_contrib.xml` 文件中的表达方式之间的相似性。

# 更多内容...

通过 Groovy 运行 Ant 的另一个示例是即时创建自定义属性文件。这允许你将信息从一个 Jenkins 任务传递到另一个任务。

你可以通过 AntBuilder 使用 echo 任务创建属性文件。以下代码行创建一个包含两行（x=1 和 y=2）的 value.properties 文件：

```
def ant = new AntBuilder()
ant.echo(message: "x=1\n", append: "false", file: "values.properties")
ant.echo(message: "y=2\n", append: "true", file: "values.properties")
```

第一个 echo 命令将 append 设置为 false，因此每次构建时都会创建一个新的属性文件。第二个 echo 命令则追加其消息。

你可以移除第二个 append 属性，因为其默认值为 true。

# 准备工作

创建一个名为 `ch3.building_software/jsp_example` 的目录。

```html
</html>
```

- 6. 使用 `mvn package` 命令创建 WAR 文件。
- 7. 通过在以 `if` 开头的行下方添加 `if (evilInput==null)` 来修改 `./src/main/webapp/index.jsp`，使其不再是一个有效的 JSP 文件。
- 8. 运行 `mvn package` 命令。现在构建将失败，并显示以下错误消息：

```text
[ERROR] Failed to execute goal org.mortbay.jetty:maven-jetty-jspc-
plugin:6.1.14:jspc (jspc) on project jsp_example: Failure processing jsps ->
[Help 1]
```

# 工作原理...

你使用一个原型（archetype）创建了一个模板项目。

Maven 插件在检测到 `index.jsp` 页面后，会将其编译成一个名为 `jsp.index_jsp` 的类，并将编译后的类放在 `WEB-INF/classes` 下。随后，插件会在 `WEB-INF/web.xml` 中将该类定义为 servlet，并映射到 `/index.jsp`。让我们看下面的示例：

```xml
<servlet>
<servlet-name>jsp.index_jsp</servlet-name>
<servlet-class>jsp.index_jsp</servlet-class>
</servlet>

<servlet-mapping>
<servlet-name>jsp.index_jsp</servlet-name>
<url-pattern>/index.jsp</url-pattern>
</servlet-mapping>
```

原型（archetype）列表会随时间不断增长。你可以在 [http://maven-repository.com/archetypes](http://maven-repository.com/archetypes) 找到完整列表。如果你运行的是 Ubuntu，你会在 `~/.m2` 目录下找到一个名为 `archetype-catalog.xml` 的本地 XML 目录文件，其中列出了所有原型。
