# # Exception handling

In a typical web application, it is required to gracefully handle exceptions thrown by controllers. When an exception is thrown by a controller, the DispatcherServlet makes use of Spring's HandlerExceptionResolver for resolving exceptions. Roo configures the SimpleMappingExceptionResolver (an implementation of the HandlerExceptionResolver) as the exception resolver for the flight-app application, as shown here:

```
<bean
    class="org.springframework.web.servlet.handler.
    SimpleMappingExceptionResolver"
    p:defaultErrorView="uncaughtException">
    <property name="exceptionMappings">
        <props>
            <prop key=".DataAccessException">dataAccessFailure</prop>
            ...
        </props>
    </property>
</bean>
```

The SimpleMappingExceptionResolver maps exception class names to error view names. In the given code, this mapping is set via the exceptionMappings property. You can either specify the fully-qualified class name of the exception or you can use a substring to match multiple exception class names to an error view. For instance, the .DataAccessException will map to the my.custom.DataAccessException as well as the org.springframework.dao.EmptyResultDataAccessException, and in both cases the DispatcherServlet will attempt to render the dataAccessFailure view. The defaultError view attribute of the SimpleMappingExceptionResolver identifies the view to which an exception is resolved if no exception mapping is found.

# ## Miscellaneous configuration

Spring Roo also configures the following classes in webmvc-config.xml:

+   * ReloadableResourceBundleMessageSource: Spring is a built-in MessageSource implementation, which loads resource bundles from WEB-INF/i18n folder.
+   * ResourceBundleThemeSource: Spring is a built-in ThemeSource implementation for loading the ResourceBundle for each theme supported by the web application. In the context of the flight-app application, this configuration loads the alt.properties and standard.properties files.
+   * CookieThemeResolver: Spring is a built-in ThemeResolver implementation, which stores a cookie in the browser for identifying the theme chosen by the user.
+   * CookieLocaleResolver: Spring is a built in LocaleResolver implementation, which stores a cookie in the browser for identifying the locale chosen by the user.