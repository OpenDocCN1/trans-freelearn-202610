# Testing the localization using Java

As you read in the previous section, the control of ResourceBundle is done using the locale. By default, the system locale is used and can be made explicit with the following source code:

```
```java
Locale locale = new Locale();
```
```

If you want to control (and change) the default locale, you can setup its value with the following source code (the example shows how to set the Spanish language):

```
```java
Locale locale = new Locale("es");
Locale.setDefault(locale);
```
```

This small piece of code is extremely useful during the tests, if you want to programmatically control the locale of your Pentaho report. In the following example, you will see how to switch from the default locale to a custom locale to test the final version.