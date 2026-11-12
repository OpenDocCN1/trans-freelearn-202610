

我们 FormulaForce 包中属于此类别的一个操作是合规性报告。该操作针对多个不同的自定义对象记录来报告合规性，例如驱动程序、汽车和车队。我们是应该定义一个针对这些物理资源各自单独应用的 URI，还是创建一个新的逻辑合规性资源？

在本章中，我选择了后者，因为这样做感觉更具扩展性，同时还利用了应用程序中的合规性服务，该服务已经支持向合规性检查器添加新的对象类型。因此，REST API 将支持新的对象类型，而无需对 API 进行未来更改，因为它们已添加到合规性检查器中。

Salesforce 规定用于定义资源的 URI 的初始部分，其余部分由开发人员定义。对于合规性 REST API，我们将使用此 URI：

/services/apexrest/compliance

一旦你打包实现此 REST API 的 Apex 代码，你会发现前面的 URI 需要由你的包命名空间限定，因此从开发人员 X 的角度来看，它将采用如下所示的格式，其中 fforce 是我的命名空间。我们将在本章后面测试合规性 REST API 时使用这种 URI 格式：

/services/apexrest/fforce/compliance

## 将 HTTP 方法映射

针对给定资源 URI 的应用程序操作应映射到 REST 基于的标准 HTTP 方法之一：GET、POST、PUT、PATCH 和 DELETE。在整个 REST API 中选择使用哪种方法并保持一致至关重要。同样，互联网上有许多资源详细讨论了此主题。以下是我对这些阅读材料的解读的简要总结：

- GET：此方法通常用于查询数据库的操作，其参数可以轻松地在 URI 结构中表达，或其参数与该方法非常匹配。

- PUT 与 POST：这些方法通常用于创建、更新或查询数据库的操作，且参数更为复杂，例如无法反映在 URI 参数中的列表或信息结构。还有一个关于 **幂等** 术语的考虑（有关此术语的完整定义，请查看 Wikipedia），它涉及请求是否可以重复多次，并在数据库中产生相同的状态。随着你深入研究 RESTful 设计准则，该术语通常决定了使用 PUT 还是 POST。

- PATCH：此方法通常用于仅更新现有信息的操作，例如某条记录或跨越多条记录的某些其他复杂数据集。

- DELETE：此方法当然用于删除 URI 中表达的现有信息。

我们将使用 `POST` HTTP 方法和 `/services/apexrest/compliance` 资源 URI 来实现合规性验证操作的 REST API，将要验证的记录列表作为 HTTP 请求体的一部分，使用 JSON 数据格式传递。

## 提供 Apex REST 应用程序 API

以下 Apex 类遵循以 `[ResourceName]Resource` 作为类名的命名约定，然后定义适当的方法（在这种情况下方法名称不那么重要），并将其相应地映射到 URL 映射的 HTTP 方法。这已包含在本章的示例代码中：

```
@RestResource(urlMapping='/compliance')
global with sharing class ComplianceResource {
  @HttpPost
  global static List<ComplianceService.VerifyResult>
  report(List<Id> Ids) {
    return ComplianceService.report(new Set<Id>(Ids));
  }
}
```

关于前面的示例，以及一般来说关于将 Service 层方法包装在 Apex REST 方法中以创建 REST API，有几点需要注意：

- 前面的示例使用 ComplianceService 中的 VerifyResult 类。你可能希望在 ComplianceResource 类范围内创建自己的类，以进一步隔离。

- 此类中的方法应根据一般 Service 层调用准则，委托给 Service 类中一个且仅一个适用的方法，以维护事务范围并关注点分离。

- 不建议将 Apex 资源类仅映射到特定的 Service 类，因为你较早的 RESTful API 设计可能与你的 Service 类设计不匹配，尽管它仍应委托给该类。

- 将这些类视为 UI 控制器来处理，仅根据需要包含封送和错误处理逻辑。

- 通常你不需要捕获来自 Service 层的异常（与 Visualforce 控制器操作方法调用者不同），因为 Apex REST 运行时会捕获异常并返回相应的 REST API 错误代码和消息。避免实现你自己的 REST API 错误处理。

- 仍然需要 `global` 修饰符。请记住，对这些方法的更改适用相同的打包限制。

- 开发人员 X 的 Apex 开发人员可以从 Apex 调用这些方法，因为它们是 `global`。但是，这不应被鼓励，因为这些方法可能引用未在标准 Apex 上下文中初始化的 REST API 上下文变量。

- `Set` Apex 数据类型不受 Apex REST 方法支持。与 Service 类方法已建立的标准不同，`Set` Apex 数据类型不受 Apex REST 方法支持。因此，你需要将 `List<Id>` REST 方法参数转换为 `Set<Id>` 集合，以供你的 Service 方法调用。

- `ComplianceService.verify` 方法会抛出包含特定合规性失败附加信息的异常。但是，自定义 Apex 异常中的信息不会由平台自动序列化——仅包含错误消息。在这种情况下，需要自定义响应来捕获异常并返回信息。

## 调用你的 Apex REST 应用程序 API

可以使用 Salesforce Developer Workbench 来调用前面的 REST API。找到未选择 FIA 超级驾驶执照字段的 Driver 记录，并使用以下 ID。请注意，以下屏幕截图中显示的 URI 包含包命名空间 fforce。要尝试此操作，你需要将此替换为你选择的包命名空间：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_375_0.png)

## 对 Apex REST 应用程序 API 进行版本控制

对 REST API 进行版本控制的公认方式是将版本包含在 URI 中。请注意，前面显示的标准 Salesforce REST API 示例中包含了 v30.0（Salesforce 平台版本），但公开合规性报告操作的 REST API 的 URI 中未包含版本。平台不提供以与其自身 API 相同的方式对 Apex REST API 进行版本控制的功能。

### 行为版本控制

Salesforce REST API 和任何公开的 Apex REST API 都可以从新的 HTTP 头中接收，例如 x-sfdc-packageVersion-fforce: 1.10。在 Apex REST API 的情况下，可以使用此头来允许打包的 API 代码实现行为版本控制。这可以用于设置本章前面所述的 System.requestVersion 返回的版本。

此头是可选的；如果没有它，则假设为当前安装的包版本。我之前关于行为版本控制利用的建议也适用于此。因为该头是可选的，所以其有用性被稀释。

### 定义版本控制

前面的 HTTP 头不以本章前面为 Apex API 所示的平台方式影响 API 定义的可见性。解决此问题的一个选项是将版本化的 URI 映射到每个版本的特定 Apex 资源类，例如，为合规性 REST API 的 v1.0 和 v2.0 版本提供支持。这种方法允许你公开包含版本的 URI：

```
/service/apexrest/fforce/v1.0/compliance
/service/apexrest/fforce/v2.0/compliance
```

在对 API 定义进行版本控制时，你还需要捕获 Service 层中使用的 Apex 数据类型的副本。这将使每个 REST API 版本与 Service 层的底层变化隔离开来，随着其演进。这里的主要编码开销将是 Service 层中定义的 Apex 类型（始终是最新版本）与实现 REST API 的类中的类型之间的封送。

