

- 5. 再次按 Tab 键，让 Roo 显示 say hello 命令的必需 name 参数，如下所示：

roo> say hello --name

- 6. 将 Ron 作为 name 参数的值，然后输入 -- 并按 Tab 键以查看 say hello 命令的可选参数：

roo> say hello --name Ron --

- 7. 由于 say hello 命令的唯一可选参数是 countryOfOrigin，因此在 Roo shell 中会显示它：

roo> say hello --name Ron --countryOfOrigin

- 8. 现在，再次按 Tab 键以查看可以传递给 countryOfOrigin 参数的参数值。您将看到以下输出：

roo> say hello --name Ron --countryOfOrigin

```
AUSTRALIA       GERMANY       NOT_SPECIFIED   UNITED_STATES
```

输出显示，countryOfOrigin 只能接受四个可能值中的一个：AUSTRALIA、GERMANY、NOT_SPECIFIED 和 UNITED_STATES。

- 9. 输入 aus 作为 countryOfOrigin 参数的值，然后按 Tab 键让 Roo 执行值的自动补全，如下所示：

roo> say hello --name Ron --countryOfOrigin aus
roo> say hello --name Ron --countryOfOrigin AUSTRALIA

如所示，Roo 为 countryOfOrigin 参数的值执行自动补全。countryOfOrigin 参数的可能值来自 MysimplePropertyName 枚举类型。

- 10. 现在，按 Enter 键让 mysimple 插件处理 say hello 命令。您将看到类似于以下内容的输出：

~.web.controller roo> say hello --name Ron --countryOfOrigin
AUSTRALIA

Welcome Ron!
Country of origin: Australia
It seems you are a running JDK 1.6.0_23
You can use the default JDK logger anywhere in your add-on to send messages to the Roo shell

执行 `say hello` 命令时，由 `MysimpleCommands` 类的 `sayHello` 方法进行处理。

- 11. 现在，执行 mysimple 插件的 `web mvc install tags`，将 `info.tagx` 和 `show.tagx` 文件安装到 `flight-app` Roo 项目中：

```
roo> web mvc install tags
```

```
Created SRC_MAIN_WEBAPP\WEB-INF\tags\util\info.tagx
Updated SRC_MAIN_WEBAPP\WEB-INF\tags\form\show.tagx
```

执行 `web mvc install tags` 的输出显示，`info.tagx` 文件已添加到 `flight-app` 项目，`show.tagx` 文件已被替换。`web mvc install tags` 命令由 `MysimpleCommands` 类的 `installTags` 方法处理，该方法又将其委托给 `MysimpleOperationsImpl` 类的 `installTags` 方法。

如果您对 mysimple 插件进行了修改并想将其重新部署到 Spring Roo，则可以使用 `osgi update` 命令，如下所示：

```
roo> osgi update --url file:///C:/roo-cookbook/ch07-simple-add-on/target/com.roo.addon.mysimple-0.1.0.BUILD-SNAPSHOT.jar
```

如果您想卸载 mysimple 插件，则可以使用 `osgi uninstall` 命令，如下所示：

```
roo> osgi uninstall --bundleSymbolicName com.roo.addon.mysimple
```

`bundleSymbolicName` 参数用于标识要从 Spring Roo 卸载的插件名称。卸载插件后，该插件公开的 Roo 命令将不再可供 Roo shell 使用。

## 带常量值的 Tab 补全功能

`MysimplePropertyName` 枚举类型定义了常量，这些常量表示 Roo 命令参数可以接受的可能值。您并不局限于使用枚举类型来定义参数值的常量。假设我们不想使用 `MySimplePropertyName` 枚举类型，而是想使用一个定义国家常量的 `Country` 类。以下代码展示了可以用来替代 `MySimplePropertyName` 枚举类型的 `Country` 类：

```
public class Country {
public static final Country AUSTRALIA =
new Country("Australia");
public static final Country NOT_SPECIFIED =
new Country("None of your business!");
public static final Country UNITED_STATES =
new Country("United States");
public static final Country GERMANY =
new Country("Germany");

private String countryName;

public Country(String countryName) {
this.countryName = countryName;
}

public String getCountryName() {
return countryName;
}
}
```

前面的代码显示，Country 类为每个国家定义了常量，这些常量代表 countryOfOrigin 参数的可能值。现在，要使用 Country 类代替 MysimplePropertyName 枚举，您只需在 MysimpleCommands 类中将对其的引用替换为 Country，如下所示：

```
public class MysimpleCommands implements CommandMarker {
@Reference private StaticFieldConverter staticFieldConverter;

protected void activate(ComponentContext context) {
staticFieldConverter.add(Country.class);
}

protected void deactivate(ComponentContext context) {
staticFieldConverter.remove(Country.class);
}
```

```
@CliCommand(value = "say hello",
help = "Prints welcome message to the Roo shell")
public void sayHello(... , @CliOption(key = "countryOfOrigin",
mandatory = false,
help = "Country of origin") Country country) {

log.info("Welcome " + name + "!");
log.warning("Country of origin: " + (country == null ?
Country.NOT_SPECIFIED.getCountryName() :
country.getCountryName())) ;
...
}
...
```

前面的代码显示，Country 类必须添加到 StaticFieldConverter 服务中，并且必须指定为 sayHello 方法中 countryOfOrigin 参数的类型。

## 使用 @CliAvailabilityIndicator 实现多个命令的可用性

在 mysimple 插件中，使用分别标注了 @CliAvailabilityIndicator 的方法来指示 say hello 和 web mvc install tags 命令的可用性。如果您想使用单个标注了 @CliAvailabilityIndicator 的方法来指示 mysimple 插件提供的多个 Roo 命令的可用性，请在 @CliAvailabilityIndicator 注解中指定命令数组。例如，您可以在 MysimpleCommands 中定义以下方法，以指示 say hello 和 web mvc install tags 命令始终可供 Roo shell 使用：

```
@CliAvailabilityIndicator({"say hello",
"web mvc install tags"})
public boolean isCommandAvailable() {
    return true;
}
```

在前面的代码中，@CliAvailabilityIndicator 注解指定了一个 Roo 命令数组（say hello 和 web mvc install tags），其可用性由 isCommandAvailable 方法进行检查。

## Roo 插件中的模板

插件项目中的模板是资源，在执行插件的一个或多个命令时，会被复制到 Roo 项目中。例如，当您执行 web mvc install tags 命令时，info.tagx 和 show.tagx 文件会从插件复制到 Roo 项目。模板也可以是图片、XML 文件、属性文件等等，由插件命令复制到 Roo 项目中。

模板位于插件项目的 SRC_MAIN_RESOURCES 目录中。例如，在 mysimple 插件的情况下，info.tagx 和 show.tagx 文件位于 SRC_MAIN_RESOURCES/com/roo/addon/mysimple 目录中。插件使用 TemplateUtils 类访问模板，然后使用 FileCopyUtils 类将其复制到 Roo 项目。TemplateUtils 定义了以下两个用于访问插件中模板的静态方法：

- String getTemplatePath(Class<?> clazz, String templateFilename)：此方法通过 templateFilename 参数返回模板文件的路径。clazz 参数的包信息用于获取 SRC_MAIN_RESOURCES 目录中包含模板文件的子目录。

例如，如果 clazz 参数表示包名为 com.roo.addon.mysimple 的类，而 templateFileName 参数的值为 show.tagx，则 getTemplatePath 方法返回 SRC_MAIN_RESOURCES/com/roo/addon/mysimple/show.tagx 文件的路径。您也可以将模板文件的相对路径作为 templateFilename 参数的值。例如，如果将 templateFilename 的值指定为 WEB-INF/myconfig.xml，则模板文件的路径变为 SRC_MAIN_RESOURCES/com/roo/addon/mysimple/WEB-INF/myconfig.xml。

- InputStream getTemplate(Class<?> clazz, String templateFileName)：此方法返回指向模板文件的 java.io.InputStream。在 mysimple 插件的情况下，MySimpleOperationsImpl 类的 createOrReplaceFile 方法使用 TemplateUtils 类来获取 info.tagx 和 show.tagx 文件的 InputStream。

在某些插件中，模板文件可能是 XML 文件，插件在将其复制到 Roo 项目之前需要对其进行修改。要修改 XML 模板，Roo 提供了 XMLUtils 类，插件可以使用它来修改 XML 模板文件的内容。让我们看一个场景，该场景展示了插件如何在将 XML 模板文件复制到 Roo 项目之前修改其内容。

以下 config.xml 文件展示了一个 Spring 应用程序上下文 XML 文件，它代表了插件的模板 XML 文件：

```
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:context="http://www.springframework.org/schema/context" ...>
<context:component-scan base-package=""/>
...
</beans>
```

在 config.xml 文件中，Spring 上下文命名空间的 <component-scan> 元素指定了通过 base-package 属性扫描的包。这些包（及其子包）中标注了 @Component、@Service 和 @Repository Spring 注解的类会自动注册到 Spring 的应用程序上下文中。由于插件在执行 Roo 命令时会将 config.xml 文件复制到 Roo 项目，因此插件事先不知道需要为 base-package 属性指定什么值。这就是 config.xml 文件中 base-package 属性的值为空的原因。

以下代码展示了插件如何读取 config.xml 文件、对其进行修改，然后将修改后的 config.xml 写入 Roo 项目：

```
import org.springframework.roo.metadata.MetadataService;
import org.springframework.roo.project.PathResolver;
import org.springframework.roo.project.ProjectMetadata;
import org.springframework.roo.support.util.XmlUtils;
import org.w3c.dom.Document;
```

