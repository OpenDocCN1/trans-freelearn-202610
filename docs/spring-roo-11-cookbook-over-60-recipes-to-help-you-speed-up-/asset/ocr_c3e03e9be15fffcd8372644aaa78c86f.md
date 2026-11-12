If you manually create controllers in a different package, then add the package containing these controllers to the base-package attribute's value.

# Annotation-driven development support and conversion service

Spring's mvc schema provides an <annotation-driven> element, which configures annotation-driven development support for Spring MVC applications, as shown here in the flight-app web application:

```
<mvc:annotation-driven
   conversion-service="applicationConversionService"/>
<bean id="applicationConversionService" class="sample.roo.flightapp.web.ApplicationConversionServiceFactoryBean"
 />
```

It is an <annotation-driven> element, which ensures that incoming requests are mapped to controllers (annotated with @Controller annotation) and to a particular @RequestMapping annotated method of the controller. The conversion-service attribute configures Spring's ConversionService where custom converters and formatters are registered. Before we go into the details of what other Spring features are configured by the <annotation-driven> element, let's take a quick look at Converter and Formatter SPIs introduced in Spring 3 for type conversion and formatting purposes.

In your application, you can either use a Converter SPI or a Formatter SPI to perform type conversions. A Converter SPI is suitable when you want to perform general-purpose type conversions from one Java type to another Java type. For instance, when you want to convert java.util.Number to java.long.Date, you can use a Converter SPI. The Formatter SPI addresses the conversion requirements typical of web applications, where you need to convert a String value to a particular Java type when an HTML form is submitted and to convert a Java type to a String value for displaying it to the user when the form is rendered. You can say that the Formatter SPI is a simplified version of the Converter SPI, and is more suitable for web application environments where localization of String values is also required.

You can access conversion and formatting functionalities using the unified ConversionService API provided by Spring. The ConversionService is backed by a registry of converters and formatters, which are applied when you perform a type conversion using ConversionService. GenericConversionService is a concrete implementation of ConversionService, which is appropriate for use in most applications. We will see later in this recipe, how the flight-app application makes use of the GenericConversionService API to perform a general-purpose type conversion at runtime.

If the conversion-service attribute is not specified, the <annotation-driven> element registers a default FormattingConversionService (which extends GenericConversionService) for performing conversions to and from java.lang.Number, java.util.Date, java.util.Calendar, and java.lang.Long. Also, full support for date and time formatting is installed if the Joda Time (http://joda-time.sourceforge.net/) library is found in the application's classpath.