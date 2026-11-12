
## How to do it...

To add new themes and modify existing themes follow the steps given here:

- 1. Create the custom.properties file in the /WEB-INF/classes folder and set the following properties:

```
styleSheet=resources/styles/custom.css
header_image=resources/images/custom_image.png
```

The custom.properties file represents the file that defines the custom theme that we want to add to the flight-app application.

- 2. Create the standard_es.properties file in the /WEB-INF/classes folder and set the following properties:

```
styleSheet=resources/styles/standard_es.css
header_image=resources/images/standard_es_image.png
```

The standard_es.properties file represents the file that defines a standard theme when the language is es.

- 3. Update the alt.properties file in the /WEB-INF/classes folder so that it has the following properties:

```
styleSheet=resources/styles/alt.css
header_image=resources/images/alt_image.png
```

The alt.properties file represents the file that defines an alternate theme. This theme is installed by Roo.

- 4. Update the standard.properties file in the /WEB-INF/classes folder so that it has the following properties:

```
styleSheet=resources/styles/standard.css
header_image=resources/images/standard_image.png
```

The standard.properties file represents the file that defines a standard theme. This theme is installed by Roo.

- 5. Copy the alt_image.png (banner image for alternate theme), the custom_image.png (banner image for newly added custom theme), the standard_es_image.png (banner image for newly added standard theme when the language is es) and the standard_image.png (banner image for standard theme) images that accompany this chapter to the SRC_MAIN_WEBAPP/images/ folder of the flight-app application.

- 6. Create a copy of SRC_MAIN_WEBAPP/styles/alt.css (CSS for alternate theme) and name it custom.css (CSS for newly added custom theme). Change the background color of the menu headings by modifying the background element of the #menu h2 definition, as shown here:

```
#menu h2 {
  color: #fff;
  background: #3f0;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 12px;
}
```

- 7. Change the background element of the #menu h2 definition in the standard.css (CSS for standard theme) file, as shown here:

```
#menu h2 {
  color: #fff;
  background: #06c;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 12px;
}
```

- 8. Create a copy of SRC_MAIN_WEBAPP/styles/standard.css and name it standard_es.css (CSS for newly added standard theme when the language is es). Change the background color of the menu headings by modifying the background element of the #menu h2 definition, as shown here:

```
#menu h2 {
  color: #fff;
  background: #f0f;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 12px;
}
```

- 9. Modify the /WEB-INF/tags/util/theme.tagx file to add the hyperlink for switching to the custom theme (or you can copy the theme.tagx file that accompanies the source code of this chapter):

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

The <spring:message> tag in the given code displays the **custom** link, which allows switching the theme to custom.

- 10. Add the following property to the /WEB-INF/i18n/messages.properties file:

```
global_theme_custom=custom
```

This property is used by the theme.tagx to display the **custom** link.

- 11. Update the /WEB-INF/views/header.jspx file to use the header_image property defined in the theme files for the banner image (or use the header.jspx file that accompanies this chapter):

```
<spring:theme code="header_image" var="headerImg"/>
<spring:url value="/${headerImg}" var="banner" />
```

- 12. Add language support for es (Español) using the web mvc install language command, as shown here:

```
..\roo> web mvc install language --code es
```

- 13. Deploy the flight-app application to embed a Tomcat instance using maven, as shown here:

```
..\roo> mvn tomcat:run
```

## How it works...

Let's take a deep dive into how themes are configured and used in Spring MVC applications:

In webmvc-config.xml, ResourceBundleThemeSource is configured by Spring Roo. When using the ResourceBundleThemeSource all theme resources (images and CSS) are defined in a properties file (a theme source), which resides in the classpath root (that is, the /WEB-INF/classes directory). So, each properties file in the /WEB-INF/classes constitutes a theme definition file. Now, by default Roo creates two properties files in the /WEB-INF/classes directory: standard.properties and alt.properties. We can say that we have two themes installed by default in the Roo-generated Spring Web MVC applications. So, what does these property files contain? Each properties file contains information about the CSS and images that form part of the theme. The alt.properties file contains the following property:

```
styleSheet=resources/styles/alt.css
```

The standard.properties file contains the following property:

```
styleSheet=resources/styles/standard.css
```

As you can see, both alt.properties and standard.properties define a styleSheet property, which refers to a CSS file. Depending upon the theme you choose in the Roo-generated web application, an appropriate style sheet is applied to the web application.

The next question that you may ask is–how the Spring Web MVC application comes to know which theme to apply? Well, this is where the following Roo-generated CookieThemeResolver configuration comes into play:

```
<bean class="org.springframework.web.servlet.theme.CookieThemeResolver" id="themeResolver" p:cookieName="theme" p:defaultThemeName="standard"/>
```

The cookieName attribute specifies the name of the cookie, which contains the theme that applies to the web application. If no cookie is found, then the configuration uses the theme identified by the defaultThemeName attribute, which happens to be standard; therefore, the theme defined by standard.properties is used by default by the Roo-generated Spring Web MVC application.

Now, we know how themes are defined using property files and how a theme is configured for Spring Web MVC application. Let's now see how theme resources are accessed by JSPX views:

JSPX views access theme resources such as images and CSS defined in property files using the <theme> tag of Spring's tag library, as shown here:

```
<spring:theme code="header_image" var="headerImg" />
```

The code attribute identifies the name of the property, which you want to access from the theme properties file. The var attribute of the <theme> tag specifies the name of the variable in which the property value is stored. The theme property file from which the property is read is dependent upon the current theme that applies to the web application. For instance, if the current theme is standard, then the header_image property is read from the standard.properties file, if the current theme is custom, then header_image property is read from the custom.properties file, and so on.

The ResourceBundleThemeSource supports configuring localized themes also. For instance, the standard_es.properties file in /WEB-INF/classes defines theme resources, which apply when the language is es and the theme is standard.

Now, coming to how you add your custom theme name at the bottom of the web page of the Roo-generated web application. All you need to do is to modify the theme.tagx file, which is responsible for showing all the theme hyperlinks. When you select the theme of your choice, the ThemeChangeInterceptor comes into picture, which allows for changing the current theme on every request.
