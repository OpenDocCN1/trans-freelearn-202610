

激活方法中指定的元数据依赖项应在元数据提供程序的停用方法中注销。元数据依赖项使用 MetadataDependencyRegistry 实例的 deregisterDependency 方法注销，MyadvancedMetadataProvider 示例如下：

```
protected void deactivate(ComponentContext context) {
    metadataDependencyRegistry.deregisterDependency(PhysicalTypeIdentifier.getMetadataIdentifierType(), getProvidesType()));
    ... }
```

## 注册和注销元数据创建触发器

元数据依赖注册可确保在上游依赖项发生更改时通知元数据的下游依赖项。要指定触发元数据创建的内容，元数据提供程序使用 AbstractItdMetadataProvider 类的 addMetadataTrigger 方法，MyadvancedMetadataProvider 示例如下：

```
protected void activate(ComponentContext context) {
    metadataDependencyRegistry.registerDependency(PhysicalTypeIdentifier.getMetadataIdentifierType(), getProvidesType()));
    addMetadataTrigger(new JavaType(RooMyadvanced.class.getName()));
}
```

在前面的代码中，addMetadataTrigger 方法接受 RooMyadvanced Java 类型。这意味着每当 Java 类型使用 @RooMyadvanced 注解进行注释时，MyadvancedMetadataProvider 将创建 MyadvancedMetadata 的实例。

元数据触发器由元数据提供程序在停用方法中移除，MyadvancedMetadataProvider 类示例如下：

```
protected void deactivate(ComponentContext context) {
    ... removeMetadataTrigger(new JavaType(RooMyadvanced.class.getName()));
}
```

现在让我们看看当 Java 类型使用 @RooMyadvanced 注解进行注释时，Spring Roo 在幕后如何生成代码。

## 插件的代码生成功能

以下时序图显示了 myadvanced 插件如何处理 myadvanced add --type sample.roo.flightapp.domain.Flight 命令，其中 Flight 类表示 Roo 项目中要添加 @RooMyadvanced 注解的 JPA 实体。

![](img/b8c379a2ed921ead3aa2b01811dc29fa_420_0.png)

该图显示，当执行 myadvanced add 命令时，会调用 MyadvancedCommands 实例的 add 方法，并将类型参数值传递给 add 方法。您可能会注意到 add 方法接受 JavaType 类型的参数，但我们没有为其向 Roo shell 注册自定义转换器。这是因为 Roo 负责将类型参数值转换为 JavaType 类型。JavaType 包含关于类型参数值的简单名称和包名称信息。在我们的例子中，传递给 add 方法的 JavaType 参数包含我们指定为类型参数值的 Flight 类的简单名称和包名称信息。MyadvancedCommands 的 add 方法调用 MyadvancedOperationsImpl 类的 annotateType 方法，并传递包含 Flight 类信息的 JavaType 实例。annotateType 方法使用 @RooMyadvanced 注解对 Flight.java 文件进行注释。

使用 @RooMyadvanced 注解对 Flight.java 进行注释会导致向 Roo 的文件监控服务发出通知，表明 Flight.java 文件已被修改。以下时序图显示了 Roo 的文件监控服务如何向 MetadataDependencyRegistry 通知 Flight.java 文件的更改：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_420_1.png)

前面的图显示，文件监控服务通知 PhysicalTypeMetadataProvider Flight.java 文件已被修改。如前面所述，PhysicalTypeMetadataProvider 为 Roo 项目中的 Java 类型提供元数据。当 Flight.java 被修改时，PhysicalTypeMetadataProvider 会收到通知，因为 PhysicalTypeMetadataProvider 是文件更改事件的注册监听器。接收到文件更改通知后，PhysicalTypeMetadataProvider 要求 MetadataDependencyRegistry 实例通知任何注册的下游依赖项。如前面所见，MyadvancedMetadataProvider 的 activate 方法将 MID:com.roo.addon.myadvanced.MyadvancedMetadata 注册为 MID:org.springframework.roo.classpath.PhysicalTypeIdentifier 的下游依赖项。MID:org.springframework.roo.classpath.PhysicalTypeIdentifier 表示 PhysicalTypeMetadataProvider 创建的类级元数据标识字符串，表示 Roo 项目中的 Java 类型。因此，Flight.java 的更改会导致通知创建 MID:com.roo.addon.myadvanced.MyadvancedMetadata 元数据标识字符串的元数据提供程序，即 MyadvancedMetadataProvider。

以下时序图显示了 MetadataDependencyRegistry 如何通知 MyadvancedMetadataProvider 创建 MyadvancedMetadata：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_421_0.png)

在前面的图中，MetadataProvider 表示 Roo 中所有元数据提供程序实现的接口，MetadataItem 表示 MetadataProvider 实现创建的元数据。对于 myadvanced 插件，MyadvancedMetadataProvider 实现 MetadataProvider 接口，MyadvancedMetadata 实现 MetadataItem 接口。

前面的时序图显示，MetadataDependencyRegistry 通知 MetadataService，以告知元数据提供程序（在 myadvanced 插件中为 MyadvancedMetadataProvider）下游依赖项。

> MetadataService 是 Roo 中的一个中心服务，了解系统中所有元数据提供程序。您无需将元数据提供程序注册到 MetadataService，因为如果实现了 MetadataProvider 接口，它可以自动将 OSGi 服务检测为元数据提供程序。

当下游依赖项的 MetadataProvider（在 myadvanced 插件中为 MyadvancedMetadataProvider）收到通知时，将创建 MetadataItem（在 myadvanced 插件中为 MyadvancedMetadata），我们稍后将看到。MetadataService 使用 MetadataIdentificationUtils 实用程序类来获取与下游依赖项的元数据标识字符串对应的 MetadataProvider 类。一旦获得下游依赖项的 MetadataProvider，MetadataService 就会通知 MetadataProvider。

MetadataProvider 需要知道要为哪个 Java 类型创建 MetadataItem。例如，在 myadvanced 插件示例中，MyadvancedMetadataProvider 需要知道需要为 Flight.java 类创建 MyadvancedMetadata。MetadataProvider 将下游依赖项的类级 MID（MID:com.roo.addon.myadvanced.MyadvancedMetadata）转换为实例级 MID（MID:com.roo.addon.myadvanced.MyadvancedMetadata #SRC_MAIN_JAVA?sample.roo.flightapp.domain.Flight），以标识要为其创建 MetadataItem 的 Java 类型。创建 MetadataItem 实例时，将生成代码。

现在让我们看看 MyadvancedMetadataProvider 的一些方法，这些方法在创建 MyadvancedMetadata 实例中起着重要作用：

```
import ...roo.classpath.itd.ItdTypeDetailsProvidingMetadataItem;
...
public final class MyadvancedMetadataProvider extends AbstractItdMetadataProvider {
...
protected ItdTypeDetailsProvidingMetadataItem getMetadata(String metadataIdentificationString,
    JavaType aspectName,
    PhysicalTypeMetadata governorPhysicalTypeMetadata,
    String itdFilename) {
    return new MyadvancedMetadata(metadataIdentificationString,
        aspectName, governorPhysicalTypeMetadata);
}
```

```
public String getItdUniquenessFilenameSuffix() {
    return "Myadvanced";
}
```

```
protected String getGovernorPhysicalTypeIdentifier(String metadataIdentificationString) {
    JavaType javaType = MyadvancedMetadata.getJavaType(metadataIdentificationString);
    Path path = MyadvancedMetadata.getPath(metadataIdentificationString);
    return PhysicalTypeIdentifier.createIdentifier(javaType, path);
}
```

```
protected String createLocalIdentifier(JavaType javaType, Path path) {
    return MyadvancedMetadata.createIdentifier(javaType, path);
}
```

```
public String getProvidesType() {
    return MyadvancedMetadata.getMetadataIdentifierType();
}
```

