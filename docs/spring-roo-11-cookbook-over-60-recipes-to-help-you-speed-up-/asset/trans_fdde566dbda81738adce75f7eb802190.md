

- ProjectOperations: 由 myadvanced 插件在 isCommandAvailable 和 setup 方法中使用，用于检查 Roo 项目是否存在并修改 Roo 项目的 pom.xml 文件，如给定代码所示。给定代码中的 isCommandAvailable 方法使用 ProjectOperations 的 isProjectAvailable 方法来确定是否已创建 Roo 项目。isCommandAvailable 方法由 MyadvancedCommands 类中带有 @CliAvailabilityIndicator 注解的方法调用。此代码中的 setup 方法使用 ProjectOperations 的 addRepository 方法将插件的仓库信息添加到 Roo 项目的 pom.xml 文件中。默认情况下，该仓库指向插件的 Google Code 仓库目录（参阅 myadvanced 插件的 pom.xml 文件）。setup 方法通过使用 addDependencies 方法，将 Roo 项目对 myadvanced 插件的依赖以及 configuration.xml（位于 SRC_MAIN_RESOURCES/com/roo/addon/myadvanced 中）文件中定义的依赖添加进去。setup 方法使用 XmlUtils 类的 getConfiguration 方法来加载 configuration.xml 文件。

- TypeLocationService: Roo 服务，用于帮助在 Roo 项目中定位 Java 类型。例如，可以使用 TypeLocationService 查找带有特定注解的 Java 类型。

- 当执行 myadvanced add 命令时，会调用 MyadvancedOperationsImpl 类的 annotateType 方法。以下代码显示了 annotateType 方法的实现：

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

annotateType 方法接受一个 JavaType 参数。该 JavaType 参数表示您在 myadvanced add 命令的 type 参数值中指定的 Java 类型。annotateType 方法执行以下操作，以使用 @RooMyadvanced 注解对 JavaType 参数进行注解：

- 获取需要应用注解的 JavaType 的元数据识别字符串。这是通过使用 Roo 的 PhysicalTypeMetadataProvider 类的 findIdentifier 方法实现的。

假设 myadvanced add 命令如下所示执行：

```
... roo> myadvanced add --type sample.roo.flightapp.domain.Flight
```

在 myadvanced add 命令中，type 参数的值为 sample.roo.flightapp.domain.Flight。此 Java 类型由 Roo 传递给 MyadvancedOperationsImpl 类的 annotateType 方法。PhysicalTypeMetadataProvider 类的 findIdentifier 方法返回的元数据识别字符串如下：

```
MID:org.springframework.roo.classpath.PhysicalTypeIdentifier#SRC_MAIN_JAVA?sample.roo.flightapp.domain.Flight
```

- 使用 Java 类型的元数据识别字符串获取 PhysicalTypeMetadata 对象，该对象表示有关 Java 类型的元数据信息。

- 使用 PhysicalTypeMetadata 的 getMemberHoldingTypeDetails 方法检索 PhysicalTypeDetails。PhysicalTypeDetails 提供 PhysicalTypeMetadata 所表示的 Java 类型的详细信息。

- 将 PhysicalTypeDetails 转换为 MutableClassOrInterfaceTypeDetails。MutableClassOrInterfaceTypeDetails 用于修改其所表示的 Java 类型，即传递给 MyadvancedOperationsImpl 类的 annotateType 方法的 JavaType 参数。除其他外，您可以使用 MutableClassOrInterfaceTypeDetails 对象从 Java 类型中添加或删除字段、方法和注解。

> 需要注意的是，从 Spring Roo 1.2.x 开始，对 Java 类型的修改是使用 Roo 的 TypeManagementService 而不是 Roo 的 MutableClassOrInterfaceTypeDetails 来执行的。

- 使用 MemberFindingUtils 工具类检查 Java 类型是否已带有 @RooMyadvanced 注解。

- 使用 AnnotationMetadataBuilder 创建 @RooMyadvanced 注解，并使用 MutableClassOrInterfaceTypeDetails 对象将其添加到 Java 类型中。

- 以下代码显示了 MyadvancedOperationsImpl 类的 annotateAll 方法，该方法将 @RooMyadvanced 注解添加到所有带有 @RooJavaBean 注解的 Java 类型上：

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

如上述代码所示，annotateAll 方法使用 TypeLocationService 查找带有 @RooJavaBean 注解的 Java 类型，然后调用 annotateType 方法以使用 @RooMyadvanced 注解对 Java 类型进行注解。

现在让我们来看看 myadvanced 插件如何使用 MyadvancedMetadataProvider 类触发代码生成。

## MyadvancedMetadataProvider 类

MyadvancedMetadataProvider 类表示一个 OSGi 组件，当 Java 类型带有 @RooMyadvanced 注解时，该组件会创建一个 MyadvancedMetadata 实例。MyadvancedMetadata 反过来会创建一个 AspectJ ITD，并向其中添加一个方法和一个字段。在本节中，我们将了解 Roo 生成的 MyadvancedMetadataProvider 是如何实现的。

以下代码显示了 MyadvancedMetadataProvider 类的一些方法：

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

上述代码显示，MyadvancedMetadataProvider 扩展了 Roo 的 AbstractItdMetadataProvider 抽象类。AbstractItdMetadataProvider 类定义了生成 AspectJ ITD 的插件所需的通用功能。activate 方法由 Apache Felix OSGi 容器在安装 myadvanced 插件时调用。

## 依赖注册和注销

在第 4 章《使用 Spring Web MVC 进行 Web 应用程序开发》中，您看到如果在 *Controller.java 类中重新定义 *_Roo_Controller.aj 文件中的方法，则 Roo 会自动从 *_Roo_Controller.aj 文件中删除该方法。我们还看到，当您修改 @RooEntity 或 @RooWebScaffold 注解时，会导致相应的 AspectJ ITD 发生修改。Roo 使用元数据识别字符串维护 Roo 项目中 Java 类型（可能是类、接口或 @Roo* 注解）之间的依赖关系，从而使 Roo 在对 Java 类型进行更改时能够管理 AspectJ ITD 中包含的代码。

现在让我们详细查看 MyadvancedMetadataProvider 类 activate 方法中的以下代码片段：

```
metadataDependencyRegistry.
    registerDependency(PhysicalTypeIdentifier.
        getMetadataIdentifierType(), getProvidesType());
```

metadataDependencyRegistry 是 Roo 的 AbstractItdMetadataProvider 类中定义的 protected 属性，类型为 MetadataDependencyRegistry。MetadataDependencyRegistry 实例跟踪元数据识别字符串之间的依赖关系。registerDependency 方法用于指定元数据识别字符串之间的依赖关系。PhysicalTypeIdentifier 表示一个 Roo 类，用于为 Roo 项目中的 Java 类型创建元数据识别字符串。

在上述代码中，PhysicalTypeIdentifier.getMetadataIdentifierType() 代码返回 MID:org.springframework.roo.classpath.PhysicalTypeIdentifier，而 MyadvancedMetadata.getMetadataIdentifierType() 返回 MID:com.roo.addon.myadvanced.MyadvancedMetadata。由于这两个元数据识别字符串都不包含其适用的 Java 类型，因此它们是类级元数据识别字符串。您可以在类级或实例级元数据识别字符串之间创建依赖关系。

MID:org.springframework.roo.classpath.PhysicalTypeIdentifier 表示上游依赖，而 MID:com.roo.addon.myadvanced.MyadvancedMetadata 表示下游依赖。当对上游依赖进行更改时，Roo 会负责通知所有下游依赖，这会导致重新创建下游元数据。MetadataProviders 负责处理通知。因此，在 myadvanced 插件的情况下，当 Roo 项目中的 Java 类型（由 MID:org.springframework.roo.classpath.PhysicalTypeIdentifier 表示）发生更改时，它会通知 MyadvancedMetadataProvider 实例。

