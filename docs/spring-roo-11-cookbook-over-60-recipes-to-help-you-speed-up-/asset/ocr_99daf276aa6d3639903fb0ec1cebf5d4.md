# Theme and locale change interceptors

As most web applications are expected to support multiple *locales* and *themes*, Roo configures Spring's `LocaleChangeInterceptor` and `ThemeChangeInterceptor` beans in the web application context XML to simplify changing locale and the theme of the web application, as shown here:

```
<mvc:interceptors>
    <bean class="org.springframework.web.servlet.theme.ThemeChangeInterceptor"/>
    <bean class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor"
        p:paramName="lang" />
</mvc:interceptors>
```

In the given code, the `<interceptors>` element of Spring's mvc schema is used to configure interceptors for pre/post-processing requests before/after the request is handled by controllers. By default, `ThemeChangeInterceptor` inspects the request parameter named `theme` to determine the theme to be applied and `LocaleChangeInterceptor` inspects the request parameter named `locale` to determine the current locale associated with the incoming request.

# ParameterizableViewController

In a Spring MVC application, controllers are responsible for processing the incoming request and returning a *logical* view name and view data. The `DispatcherServlet` hands over the logical view name to Spring's `ViewResolver` (configured in the web application context), which resolves the actual view corresponding to the logical view name. The `DispatcherServlet` then renders the actual view. So, does it mean that even if a web application needs to show a static web page, we will need to create a Spring MVC controller for it? Well, this is where the `ParameterizableViewController` built-in controller of Spring MVC comes into picture. The `ParameterizableViewController` simply returns a *pre-configured* view name, which is resolved by the `ViewResolver` and rendered by the `DispatcherServlet`. This saves the effort of creating a custom controller, which does nothing but return the name of view to be rendered. You can either directly configure a `ParameterizableViewController` in your web application context or use Spring's mvc schema's `<view-controller>` element to do it for you.

In case of our `flight-app` application, Roo makes use of the `<view-controller>` element for static views, such as the home page of the `flight-app` application and the various exception pages, as shown here:

```
<mvc:view-controller path="/" view-name="index" />
<mvc:view-controller path="/uncaughtException" />
```

In the `<view-controller>` element the `view-name` attribute identifies the name of the view mapped to the URL path identified by the `path` attribute. So, if the URL used to access the `flight-app` application is `http://localhost:8080/flight-app/`, the view name corresponding to this URL is `index`.