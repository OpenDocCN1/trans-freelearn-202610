
## 包扩展 – 为多个产品和版本定制应用程序

包扩展为 AppExchange 合作伙伴提供了一种灵活的方法，用于构建面向使用不同 Salesforce 产品（如 Health Cloud 和 Financial Services Cloud）以及不同 Salesforce 版本（如 Professional）的客户的应用程序。通过使用基础托管包和扩展包，您可以创建一个模块化应用程序，满足多样化的客户需求，而无需维护多个代码库。

将您的应用程序视为一个多面拼图，其中基础托管包构成中心拼块，形成适用于所有客户的核心功能。另一方面，扩展包就像专门的拼图块，可以通过附加功能和功能增强基础包，并针对特定的 Salesforce 产品或版本进行定制。

在 2GP 的上下文中，您可以创建一个包含核心功能的 2GP 基础包，然后开发一个或多个依赖于基础包的扩展 2GP 包。根据组织结构和需求，这些扩展包可以包含分层功能，从数据模型和逻辑到 UI。这些包还可能包含特定于某些 Salesforce 产品或版本的功能，使合作伙伴能够有效地探索和扩展其产品。这种模块化方法使您能够维护单一代码库，同时为不同客户提供定制解决方案。

在设计应用程序和包之间的接口时，请考虑整个应用程序生命周期。必须确保对基础包的更新不会破坏扩展包。在添加新功能时，将其包含在适当的包中，并在扩展包引用的情况下使必要的组件成为 `@namespaceAccessible` 组件。

利用 2GP 中的包扩展，您可以创建可扩展且模块化的应用程序，以满足多样化客户的需求，同时简化升级过程。随着客户需求的增长，您可以通过引入适当的扩展包来增强其应用程序体验，从而轻松地向他们进行 upsell。

## 包扩展与功能参数

在 Salesforce AppExchange 市场上开发时，您可能会经常面临两难选择：应该构建扩展包还是使用 **功能管理应用 (FMA)**，所述于 *第 2 章*？这两种方法各有其优势和用途。以下是实用的比较：

| Criteria | Package extensions | Feature parameters |
|---|---|---|
| Scope | These are designed to extend the functionality of a base package. They have a dependency on the base package and can’t be installed independently. | This is more about controlling the visibility and access to specific features within a package. It doesn’t add new functionality but rather manages existing ones. |
| Flexibility | This approach provides a great deal of flexibility because it enables you to add new objects, fields, logic, and dependencies on Salesforce features that are not required by your base package. It’s like building a new app but with a dependency on the base package. | Limited to toggling features on or off. New components must be added to the base package. |
| Use case | Ideal for scenarios where you have different versions of your app for different industries or when you have add-on modules that not every customer might need. | This approach is ideal for beta testing features or for gradually introducing new features to your customers. It also allows for control over usage allowance allocations and the disabling of features in a freemium or tiered-access model. |
|  | Requires more development and testing effort as you’re essentially building a new (albeit smaller) app. | Easier to implement as you’re just controlling access to existing features. |
| Complexity |  |  |
| Technical debt | Can reduce technical debt due to better **separation of concerns** (**SoC**). | Can increase technical debt due to added complexity in the core code base. |

功能参数特别适合于渐进式功能部署，通过 A/B 测试获得洞察，允许最终用户动态自定义产品功能，并在增强与核心产品紧密相关时确保高效维护。重要的是，它们不会引入维护和发布单独包的开销，使其成为在核心包内管理功能的简化解决方案。然而，这种方法虽然在功能演进方面提供了灵活性，但会增加核心代码库的复杂性。

相反，包扩展旨在通过创建单独的包来增强现有产品的功能。当需要模块化功能、管理可能干扰核心产品的复杂性，或新功能需要不同的版本和发布节奏时，它们是首选。包扩展虽然促进了更好的 SoC 并由于其模块化特性而减少了技术债务，但也带来了一些挑战。它们需要创建、维护和发布单独的包，这可能会带来额外的开销。其隔间化设计确保了更干净的代码库，但需要更严格的管理。

在做出决定时，您应权衡产品的性质、其战略目标和技术挑战。虽然包扩展适用于模块化和复杂的增强，但功能参数具有适应性和多功能性。最终决定应与您的独特轨迹和目标产生共鸣。

## Dev Hub

Dev Hub 是 Salesforce 开发人员和 AppExchange 合作伙伴的重要工具，提供集中环境来管理、开发和部署 Salesforce 应用程序，特别是 2GP 托管包。在本节中，我们将讨论 Dev Hub 与 2GP 托管包之间的关系，强调您需要了解的关键信息。

2GP 打包模型中的托管包由 Dev Hub 拥有，但打包的元数据并不存储在其中。要使用 2GP 包，建议在您的 **合作伙伴业务组织 (PBO)** 中启用 Dev Hub。在 2GP 打包模型中，单个 Dev Hub 实例可以拥有一个或多个包，从而简化多个应用程序的管理流程。

建议在单独的 Developer Edition 组织中注册命名空间，然后将其链接到您的 Dev Hub 实例。此过程允许您使用命名空间并将多个命名空间与单个 Dev Hub 实例关联，从而增强组织和灵活性。要实现此目的，您需要执行以下操作：

- 1. 如果您还没有带有已注册命名空间的组织，请创建一个单独的 Developer Edition 组织。
- 2. 在 Developer Edition 组织中注册命名空间。选择命名空间时要谨慎，因为一旦与组织关联，就无法更改或重用。
- 3. 以系统管理员或具有 Salesforce DX 命名空间注册权限的用户身份登录到您的 Dev Hub 组织。
- 4. 在 Dev Hub 组织的 **App Launcher** 菜单中，选择 **Namespace Registries**。
- 5. 单击 **Link Namespace** 并使用系统管理员凭据登录到 Developer Edition 组织。

请注意，在您可以链接命名空间之前，有必要在 Dev Hub 组织中定义并部署 **My Domain** 名称（如果尚未完成）。

## 托管包的本地化最佳实践

在动态的 Salesforce ISV 开发世界中，一切都应实现本地化。这包括从屏幕流文本、自定义选项卡、对象和字段到可能向用户展示的异常消息的所有内容。

## 管理冗余和依赖关系

您可能会在标签中遇到冗余，尤其是在应用程序较大时。然而，不必对此过于担心。同一短语在不同语言中可能有不同的含义，而在一种语言中似乎冗余的内容在另一种语言中可能并非如此。因此，将每个标签视为唯一并为其提供适当的上下文是有益的。

## 模块化标签和版本控制

标签理想情况下应保存在每个包的单个 `CustomLabels` XML 文件中，按 API 名称按字母顺序维护。您应手动将新标签合并到包元数据中，具体在 `CustomLabels.labels-meta.xml` 文件中。这需要使用 **集成开发环境 (IDE)**，而不是依赖 Salesforce UI。

在每个标签 API 名称上使用前缀来定义其所属的模块，并使用类别对标签进行分组，可以进一步帮助组织。

## 翻译工具

翻译过程可以外包给专业的翻译服务。将标签导出为 XLF 格式可以简化他们的流程，他们将维护已翻译内容的目录，使每次识别新条目或修改条目变得更容易。

## 允许订阅者调整您的翻译

在命名空间包中，最终客户可以覆盖自定义对象的元数据翻译。例如，他们可以使用 Translation Workbench 覆盖自定义字段或工作流任务上的标签。如果他们这样做，对应用程序的更新将不会覆盖本地翻译。虽然您
