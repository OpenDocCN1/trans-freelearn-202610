
- ProjectOperations: this is used by the myadvanced add-on in the isCommandAvailable and setup methods to check if a Roo project exists and to modify the pom.xml file of the Roo project, as shown in the given code. The isCommandAvailable method in the given code makes use of the isProjectAvailable method of ProjectOperations to determine if a Roo project has been created. The isCommandAvailable method is invoked by the @CliAvailabilityIndicator annotated method of the MyadvancedCommands class. The setup method in this code makes use of the addRepository method of ProjectOperations to add repository information of the add-on to the Roo project's pom.xml file. By default, the repository refers to the repo directory of the Google Code of the add-on (refer to the pom.xml file of the myadvanced add-on). The setup method adds Roo project's dependency on the myadvanced add-on and the dependencies defined in the configuration.xml (inside SRC_MAIN_RESOURCES/com/roo/addon/myadvanced) file by using the addDependencies method. The setup method makes use of the getConfiguration method of XmlUtils class to load the configuration.xml file.

- TypeLocationService: A Roo service that helps with locating Java types in the Roo project. For instance, you can find Java types annotated with a particular annotation using TypeLocationService.

- The annotateType method of the MyadvancedOperationsImpl class is invoked when the myadvanced add command is executed. The following code shows the implementation of the annotateType method:

```
import ...roo.classpath.PhysicalTypeDetails;
import ...roo.classpath.PhysicalTypeMetadata;
import ...roo.classpath.PhysicalTypeMetadataProvider;
import ...roo.classpath.TypeLocationService;
import ...roo.classpath.details.MemberFindingUtils;
import ...roo.classpath.details.MutableClassOrInterfaceTypeDetails;
import ...roo.classpath.details.annotations.AnnotationMetadataBuilder;
...
@Reference private MetadataService metadataService;
@Reference private PhysicalTypeMetadataProvider
        physicalTypeMetadataProvider;
...
public void annotateType(JavaType javaType) {
        String id =
                physicalTypeMetadataProvider.findIdentifier(javaType);
        PhysicalTypeMetadata physicalTypeMetadata =
                (PhysicalTypeMetadata) metadataService.get(id);
        PhysicalTypeDetails physicalTypeDetails =
                physicalTypeMetadata.getMemberHoldingTypeDetails();
        MutableClassOrInterfaceTypeDetails mutableTypeDetails =
                (MutableClassOrInterfaceTypeDetails) physicalTypeDetails;
        if (MemberFindingUtils.getAnnotationOfType(
                mutableTypeDetails.getAnnotations(),
                new JavaType(RooMyadvanced.class.getName())) == null) {
            JavaType rooRooMyadvanced =
                    new JavaType(RooMyadvanced.class.getName());
            AnnotationMetadataBuilder annotationBuilder =
                    new AnnotationMetadataBuilder(rooRooMyadvanced);
            mutableTypeDetails.addTypeAnnotation(
                    annotationBuilder.build());
        }
}
```

The annotateType method accepts a JavaType argument. The JavaType argument represents the Java type that you specified as the value of the type argument of the myadvanced add command. The annotateType method performs the following actions to annotate the JavaType argument with the @RooMyadvanced annotation:

- Obtains metadata identification string for the JavaType on which the annotation needs to be applied. This is achieved by using the findIdentifier method of the PhysicalTypeMetadataProvider class of Roo.

Let's say that the myadvanced add command is executed as shown here:

```
... roo> myadvanced add --type sample.roo.flightapp.domain.Flight
```

In the myadvanced add command, the value of the type argument is sample.roo.flightapp.domain.Flight. This Java type is passed to the annotateType method of the MyadvancedOperationsImpl class by Roo. The metadata identification string returned by the findIdentifier method of PhysicalTypeMetadataProvider is as follows:

```
MID:org.springframework.roo.classpath.PhysicalTypeIdentifier#SRC_MAIN_JAVA?sample.roo.flightapp.domain.Flight
```

- Uses the metadata identification string of the Java type to obtain the PhysicalTypeMetadata object, which represents the metadata information about the Java type.

- Makes use of the getMemberHoldingTypeDetails method of PhysicalTypeMetadata to retrieve PhysicalTypeDetails. The PhysicalTypeDetails provides details of the Java type represented by PhysicalTypeMetadata.

- Casts the PhysicalTypeDetails into MutableClassOrInterfaceTypeDetails. The MutableClassOrInterfaceTypeDetails is used to modify the Java type represented by it, which is the JavaType argument passed to the annotateType method of MyadvancedOperationsImpl class. Amongst other things, you can use the MutableClassOrInterfaceTypeDetails object to add or remove fields, methods, and annotations from the Java type.

> It is important to note that starting with Spring Roo 1.2.x, modifications to a Java type are performed using Roo's TypeManagementService instead of Roo's MutableClassOrInterfaceTypeDetails.

- Uses MemberFindingUtils utility class to check if the Java type is already annotated with the @RooMyadvanced annotation.

- Uses AnnotationMetadataBuilder to create the @RooMyadvanced annotation, and adds it to the Java type using the MutableClassOrInterfaceTypeDetails object.

- The following code shows the annotateAll method of the MyadvancedOperationsImpl class, which adds the @RooMyadvanced annotation to all the Java types annotated with the @RooJavaBean annotation:

```
import ...roo.classpath.TypeLocationService;
import ...roo.model.JavaType;
...
public void annotateAll() {
    for (JavaType type:
        typeLocationService.findTypesWithAnnotation(
        new JavaType("org.springframework.roo.addon. " +
        "javabean.RooJavaBean"))) {
        annotateType(type);
    }
}
```

As shown in the preceding code, the annotateAll method makes use of TypeLocationService to find Java types that are annotated with the @RooJavaBean annotation, and then invokes the annotateType method to annotate Java types with the @RooMyadvanced annotation.

Let's now look at how the myadvanced add-on triggers code generation using the MyadvancedMetadataProvider class.

## The MyadvancedMetadataProvider class

The MyadvancedMetadataProvider class represents an OSGi component which creates a MyadvancedMetadata instance when a Java type is annotated with the @RooMyadvanced annotation. The MyadvancedMetadata in turn creates an AspectJ ITD and adds a method and a field to it. In this section, we'll look at how Roo-generated MyadvancedMetadataProvider is implemented.

The following code shows some of the methods of the MyadvancedMetadataProvider class:

```
import org.apache.felix.scr.annotations.Component;
import org.apache.felix.scr.annotations.Service;
import org.osgi.service.component.ComponentContext;
import ...roo.classpath.PhysicalTypeIdentifier;
import ...roo.classpath.PhysicalTypeMetadata;
import ...roo.classpath.itd.AbstractItdMetadataProvider;
import ...roo.classpath.itd.ItdTypeDetailsProvidingMetadataItem;
import org.springframework.roo.model.JavaType;
import org.springframework.roo.project.Path;

@Component
@Service
public final class MyadvancedMetadataProvider extends
AbstractItdMetadataProvider {

protected void activate(ComponentContext context) {
metadataDependencyRegistry.
registerDependency(PhysicalTypeIdentifier.
getMetadataIdentifierType(), getProvidesType() );

addMetadataTrigger(
new JavaType(RooMyadvanced.class.getName() ));
}

protected void deactivate(ComponentContext context) {
metadataDependencyRegistry.
deregisterDependency(PhysicalTypeIdentifier.
getMetadataIdentifierType(), getProvidesType() );

removeMetadataTrigger(
new JavaType(RooMyadvanced.class.getName() ));
}
...
}
```

The preceding code shows that MyadvancedMetadataProvider extends Roo's AbstractItdMetadataProvider abstract class. The AbstractItdMetadataProvider class defines the common functionality required by add-ons that generate AspectJ ITDs. The activate method is invoked by the Apache Felix OSGi container when the myadvanced add-on is installed.

## Dependency registration and unregistration

In Chapter 4, Web Application Development with Spring Web MVC you saw that if you redefine a method of the *_Roo_Controller.aj file in the *Controller.java class, then Roo automatically removes that method from the *_Roo_Controller.aj file. We also saw that when you modify a @RooEntity or @RooWebScaffold annotation, it results in modification of corresponding AspectJ ITDs. Roo maintains dependency between Java types (which could be a class, interface, or a @Roo* annotation) of the Roo project using metadata identification strings, making it possible for Roo to manage code contained in AspectJ ITD when changes are made to a Java type.

Let's now look closely at the following code snippet in the activate method of MyadvancedMetadataProvider class:

```
metadataDependencyRegistry.
    registerDependency(PhysicalTypeIdentifier.
        getMetadataIdentifierType(), getProvidesType());
```

The metadataDependencyRegistry is a protected attribute defined in Roo's AbstractItdMetadataProvider class and is of type MetadataDependencyRegistry. The MetadataDependencyRegistry instance keeps track of dependencies between metadata identification strings. The registerDependency method is used to specify dependency between metadata identification strings. PhysicalTypeIdentifier represents a Roo class that creates a metadata identification string for a Java type in Roo project.

In the previous code, the PhysicalTypeIdentifier.getMetadataIdentifierType() code returns MID:org.springframework.roo.classpath.PhysicalTypeIdentifier, and MyadvancedMetadata.getMetadataIdentifierType() returns MID:com.roo.addon.myadvanced.MyadvancedMetadata. As both the metadata identification strings don't contain the Java type to which they apply, they are class-level metadata identification strings. You can create dependencies between class level-or instance-level metadata identification strings.

The MID:org.springframework.roo.classpath.PhysicalTypeIdentifier represents the upstream dependency and MID:com.roo.addon.myadvanced.MyadvancedMetadata represents the downstream dependency. When changes are made to an upstream dependency, Roo takes care of notifying all the downstream dependencies, which results in recreating the downstream metadata. The MetadataProviders are responsible for handling the notification. So, in the case of the myadvanced add-on, when a Java type (represented by MID:org.springframework.roo.classpath.PhysicalTypeIdentifier) in the Roo project is changed, it notifies the MyadvancedMetadataProvider instance.
