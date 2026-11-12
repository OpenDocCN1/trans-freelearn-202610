The preceding code shows that MyadvancedMetadataProvider extends Roo's AbstractIdMetadataProvider abstract class. The AbstractIdMetadataProvider class defines the common functionality required by add-ons that generate AspectJ ITDs. The activate method is invoked by the Apache Felix OSGi container when the myadvanced add-on is installed.

# Dependency registration and unregistration

In Chapter 4, Web Application Development with Spring Web MVC you saw that if you redefine a method of the *_Roo_Controller.aj file in the *Controller.java class, then Roo automatically removes that method from the *_Roo_Controller.aj file. We also saw that when you modify a @RooEntity or @RooWebScaffold annotation, it results in modification of corresponding AspectJ ITDs. Roo maintains dependency between Java types (which could be a class, interface, or a @Roo* annotation) of the Roo project using metadata identification strings, making it possible for Roo to manage code contained in AspectJ ITD when changes are made to a Java type.

Let's now look closely at the following code snippet in the activate method of MyadvancedMetadataProvider class:

```
metadataDependencyRegistry.
    registerDependency(PhysicalTypeIdentifier.
        getMetadataIdentifierType(), getProvidesType());
```

The metadataDependencyRegistry is a protected attribute defined in Roo's AbstractIdMetadataProvider class and is of type MetadataDependencyRegistry. The MetadataDependencyRegistry instance keeps track of dependencies between metadata identification strings. The registerDependency method is used to specify dependency between metadata identification strings. PhysicalTypeIdentifier represents a Roo class that creates a metadata identification string for a Java type in Roo project.

In the previous code, the PhysicalTypeIdentifier.getMetadataIdentifierType() code returns MID:org.springframework.roo.classpath.PhysicalTypeIdentifier, and MyadvancedMetadata.getMetadataIdentifierType() returns MID:com.roo.addon.myadvanced.MyadvancedMetadata. As both the metadata identification strings don't contain the Java type to which they apply, they are class-level metadata identification strings. You can create dependencies between class level-or instance-level metadata identification strings.

The MID:org.springframework.roo.classpath.PhysicalTypeIdentifier represents the upstream dependency and MID:com.roo.addon.myadvanced.MyadvancedMetadata represents the downstream dependency. When changes are made to an upstream dependency, Roo takes care of notifying all the downstream dependencies, which results in recreating the downstream metadata. The MetadataProviders are responsible for handling the notification. So, in the case of the myadvanced add-on, when a Java type (represented by MID:org.springframework.roo.classpath.PhysicalTypeIdentifier) in the Roo project is changed, it notifies the MyadvancedMetadataProvider instance.