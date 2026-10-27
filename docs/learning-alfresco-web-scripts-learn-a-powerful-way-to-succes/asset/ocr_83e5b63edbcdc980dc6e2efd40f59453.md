If you try to register the web script without providing the shortname value, it will not get registered. The following is the snippet of the error that will appear in the logs:

> **Unable to register script workspace://SpacesStore/app:company_home/app:dictionary/cm:extensionwebscripts/helloworld.get.desc.xml due to error: Failed to parse web script description document helloworld.get.desc.xml; Expected <shortname> value.**

# ## The `<url>` tag

Each web script is bound to a URI that will be used to invoke the web script. It is possible to have one or more URI declaration for a web script, and any of the URIs can be used to execute the web scripts. This tag does not have any attributes:

```
```xml
<url>/helloworld?name={argumentName}</url>
<url>/hello/world/{argumentName}</url>
```
```

If a URL is not provided in the description document and you try to register the web script, it will give an error and will not register the web script. The following is the snippet of the error from the logs when there isn't any URL provided in the web script description document:

> **Unable to register script workspace://SpacesStore/app:company_home/app:dictionary/cm:extensionwebscripts/helloworld.get.desc.xml due to error: Failed to parse web script description document helloworld.get.desc.xml; Expected at least one <url> element.**

If a URL has been defined in one web script and you try to define the same URL in another web script, then the web script engine will not register the web script and an error as shown in the following example will appear in the logs:

> **Unable to register script classpath:alfresco/extension/templates/webscripts/test/helloworld1.get.desc.xml due to error: Web Script document test/helloworld1.get.desc.xml is attempting to define the url '/helloworld1:GET' already defined by test/helloworld.get.desc.xml.**

A minimal description file with all the mandatory elements for a hello world web script will appear as follows:

```
```xml
<webscript>
    <shortname>Hello World</shortname>
    <url>/helloworld?name={argumentName}</url>
    <url>/hello/world/{argumentName}</url>
</webscript>
```
```