
## 如何执行...

要添加新主题并修改现有主题，请按照此处给出的步骤操作：

- 1. 在 /WEB-INF/classes 文件夹中创建 custom.properties 文件并设置以下属性：

```
styleSheet=resources/styles/custom.css
header_image=resources/images/custom_image.png
```

custom.properties 文件表示定义我们要添加到 flight-app 应用程序的自定义主题的文件。

- 2. 在 /WEB-INF/classes 文件夹中创建 standard_es.properties 文件并设置以下属性：

```
styleSheet=resources/styles/standard_es.css
header_image=resources/images/standard_es_image.png
```

standard_es.properties 文件表示定义语言为 es 时的标准主题的文件。

- 3. 更新 /WEB-INF/classes 文件夹中的 alt.properties 文件，使其包含以下属性：

```
styleSheet=resources/styles/alt.css
header_image=resources/images/alt_image.png
```

alt.properties 文件表示定义备用主题的文件。该主题由 Roo 安装。

- 4. 更新 /WEB-INF/classes 文件夹中的 standard.properties 文件，使其包含以下属性：

```
styleSheet=resources/styles/standard.css
header_image=resources/images/standard_image.png
```

standard.properties 文件表示定义标准主题的文件。该主题由 Roo 安装。

- 5. 将随本章附带的 alt_image.png（备用主题横幅图片）、custom_image.png（新添加自定义主题横幅图片）、standard_es_image.png（语言为 es 时新添加标准主题横幅图片）和 standard_image.png（标准主题横幅图片）复制到 flight-app 应用程序的 SRC_MAIN_WEBAPP/images/ 文件夹中。

- 6. 复制 SRC_MAIN_WEBAPP/styles/alt.css（备用主题的 CSS）并将其命名为 custom.css（新添加自定义主题的 CSS）。通过修改 #menu h2 定义中的 background 元素来更改菜单标题的背景颜色，如下所示：

```
#menu h2 {
  color: #fff;
  background: #3f0;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 12px;
}
```

- 7. 更改 standard.css（标准主题 CSS）文件中 #menu h2 定义的 background 元素，如下所示：

```
#menu h2 {
  color: #fff;
  background: #06c;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 12px;
}
```

- 8. 复制 SRC_MAIN_WEBAPP/styles/standard.css 并将其命名为 standard_es.css（语言为 es 时新添加标准主题的 CSS）。通过修改 #menu h2 定义中的 background 元素来更改菜单标题的背景颜色，如下所示：

```
#menu h2 {
  color: #fff;
  background: #f0f;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 12px;
}
```

- 9. 修改 /WEB-INF/tags/util/theme.tagx 文件以添加切换到自定义主题的超链接（或者，您也可以复制随本章源代码附带的 theme.tagx 文件）：

```
...
<c:out value=" | " />
<spring:url var="url_theme3" value="">
  <spring:param name="theme" value="custom" />
  <c:if test="${not empty param.page}">
    <spring:param name="page" value="${param.page}" />
  </c:if>
  <c:if test="${not empty param.size}">
    <spring:param name="size" value="${param.size}" />
  </c:if>
</spring:url>

<spring:message code="global_theme_custom" var="theme_custom" />

<a href="${url_theme3}" title="${theme_custom}">${theme_custom}</a>
```

给定代码中的 <spring:message> 标签显示 **自定义** 链接，该链接允许将主题切换到 custom。

- 10. 将以下属性添加到 /WEB-INF/i18n/messages.properties 文件中：

```
global_theme_custom=custom
```

此属性由 theme.tagx 用于显示 **自定义** 链接。

- 11. 更新 /WEB-INF/views/header.jspx 文件以使用主题文件中定义的 header_image 属性作为横幅图片（或者，使用随本章附带的 header.jspx 文件）：

```
<spring:theme code="header_image" var="headerImg"/>
<spring:url value="/${headerImg}" var="banner" />
```

- 12. 使用 web mvc install language 命令添加对 es（西班牙语）的语言支持，如下所示：

```
..\roo> web mvc install language --code es
```

- 13. 使用 Maven 将 flight-app 应用程序部署到嵌入式 Tomcat 实例中，如下所示：

```
..\roo> mvn tomcat:run
```

## 工作原理...

让我们深入了解 Spring MVC 应用程序中主题的配置和使用方式：

在 webmvc-config.xml 中，ResourceBundleThemeSource 由 Spring Roo 配置。使用 ResourceBundleThemeSource 时，所有主题资源（图像和 CSS）都定义在一个属性文件（主题源）中，该文件位于类路径根目录（即 /WEB-INF/classes 目录）中。因此，/WEB-INF/classes 中的每个属性文件都构成一个主题定义文件。默认情况下，Roo 在 /WEB-INF/classes 目录中创建两个属性文件：standard.properties 和 alt.properties。可以说，在 Roo 生成的 Spring Web MVC 应用程序中，默认安装了两个主题。那么，这些属性文件包含什么内容？每个属性文件都包含构成主题一部分的 CSS 和图像的信息。alt.properties 文件包含以下属性：

```
styleSheet=resources/styles/alt.css
```

standard.properties 文件包含以下属性：

```
styleSheet=resources/styles/standard.css
```

如您所见，alt.properties 和 standard.properties 均定义了 styleSheet 属性，该属性引用一个 CSS 文件。根据您在 Roo 生成的 Web 应用程序中选择的主题，将应用适当的样式表到 Web 应用程序。

您可能会问的下一个问题是——Spring Web MVC 应用程序如何知道应用哪个主题？好吧，这时以下 Roo 生成的 CookieThemeResolver 配置就派上用场了：

```
<bean class="org.springframework.web.servlet.theme.CookieThemeResolver" id="themeResolver" p:cookieName="theme" p:defaultThemeName="standard"/>
```

cookieName 属性指定 cookie 的名称，该 cookie 包含应用于 Web 应用程序的主题。如果未找到 cookie，则配置使用 defaultThemeName 属性标识的主题（即 standard）；因此，由 standard.properties 定义的主题默认由 Roo 生成的 Spring Web MVC 应用程序使用。

现在，我们知道如何使用属性文件定义主题以及如何为 Spring Web MVC 应用程序配置主题。现在让我们看看 JSPX 视图如何访问主题资源：

JSPX 视图使用 Spring 标签库的 <theme> 标签访问属性文件中定义的主题资源（例如图像和 CSS），如下所示：

```
<spring:theme code="header_image" var="headerImg" />
```

code 属性用于标识您要从主题属性文件中访问的属性名称。<theme> 标签的 var 属性指定存储属性值的变量名称。读取属性所依据的主题属性文件取决于应用于 Web 应用程序的当前主题。例如，如果当前主题是 standard，则从 standard.properties 文件中读取 header_image 属性；如果当前主题是 custom，则从 custom.properties 文件中读取 header_image 属性，以此类推。

ResourceBundleThemeSource 还支持配置本地化主题。例如，/WEB-INF/classes 中的 standard_es.properties 文件定义了主题资源，当语言为 es 且主题为 standard 时应用这些资源。

现在，让我们来看看如何在 Roo 生成的 Web 应用程序的网页底部添加自定义主题名称。您只需修改 theme.tagx 文件，该文件负责显示所有主题超链接。当您选择心仪的主题时，ThemeChangeInterceptor 就会介入，它允许在每个请求中更改当前主题。
