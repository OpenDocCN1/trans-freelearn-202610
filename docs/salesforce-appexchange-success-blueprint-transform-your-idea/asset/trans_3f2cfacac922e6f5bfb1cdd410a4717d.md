
为满足此类需求，最优解决方案是设计一个配置向导。通常，这是一个简单的 Lightning Web Components (LWC) 界面，为管理员提供更新或配置必要细节的途径，而无需访问完整的元数据。有了此界面，管理员提供必要的数据，然后您的 Apex 代码会安全地将这些信息保存到受保护的自定义元数据中。

## Security pitfalls

虽然配置向导为管理员提供了增强的灵活性，但仍需谨慎注意潜在的安全陷阱。开发人员和管理员应了解的一个关键问题是被称为“password echo”的漏洞。密码或任何敏感数据在输入后绝不应回显给用户，因为这是存储不安全的一个明显迹象。理想情况下，密码应使用涉及哈希和加盐的方法进行存储，确保即使系统本身也不知道明文版本，从而使密码回显实际上不可能。此外，包含敏感数据的字段，例如 **Consumer Key** 和 **Consumer Secret**，应保持隐藏。管理员在配置向导中首次输入时可能需要查看这些值，但在后续交互或编辑过程中不应能够访问它们。此外，务必始终警惕数据的记录位置。确保敏感细节不会意外出现在 debug logs、JavaScript logs 或其他非预期存储点中，对于维护系统的安全完整性至关重要。此类漏洞可能是导致安全审查失败的原因，我们将在下一章深入探讨这一主题。

## Utilizing Salesforce APIs for inbound integrations

你可能在想，API 到底是什么？好吧，让我们拆解一下。你可以把 Application Programming Interface（API）想象成一个勤奋而有礼的管家，总是准备好将你的消息（请求）从一个房间（一个应用程序）递送到另一个房间（服务器），取回答案再带回来给你。这一切都很有《唐顿庄园》的感觉，只是我们面对的不是 Dowager Countess，而是数据。

在 Salesforce 领域，API 就像高效的桥梁建造者，在你的 Salesforce Org 与其他功能之间构建坚固的连接，拓展你在商业生态系统中的版图。

## Types of Salesforce APIs

好吧，Salesforce 拥有一个完整的 API 自助餐——每种都有其独特的风味和用途。这简直就是一场 API 拼盘聚会！

SOAP and REST APIs：可将它们视为 Salesforce 提供的每项丰富 API 功能的基础。它们就像 API 世界中必不可少的面包和黄油。

Connect REST API：这类似于为移动应用程序提供的个人礼宾服务，擅长从推送通知到展示你的 Chatter 动态等一切事务。

Metadata API：它为你提供进入 Salesforce Org 后台的通道。它处理设置的方式就像舞台经理一样——管理自定义和构建工具，而不是舞台设备。

User Interface API：此 API 让你可以编排一场独特的舞台表演——在这种情况下，打造一个按你设想方式呈现 Salesforce 数据或元数据的用户界面。

GraphQL API：将其想象为 API 乐团的指挥，通过允许客户端请求所需内容来协调和谐的交互。GraphQL 作为单一端点，根据查询动态塑造响应，而不是多个端点返回固定的数据结构。

Bulk API：想象它是一个超级英雄助手，能够在一举之间处理庞大的数据量并以强力扫荡的方式操作记录。

Streaming API：这是你的专属新闻播报员，提供实时更新流，让你随时了解 Salesforce Org 中每一次细微变化。

Tooling API：当复杂元数据类型需要专家之手时，Tooling API 便会介入，以精湛的技巧处理它们。

Analytics REST API：最后，Analytics API 充当你的可靠间谍，深入特定数据集、筛选器和仪表板以获取宝贵的情报。

## Choosing the right Salesforce API for integration

![](img/44ed632038957e4c80220676e84c5109_264_0.png)

选择 API 有点像速配活动——你希望能迅速找到合适的匹配。你需要实时查看数据变化吗？Streaming API 听起来不错。处理海量数据？Bulk API 可能是你的英雄。如果你想把 Salesforce Apex 代码开放给世界，Apex SOAP 和 REST API 可能就会俘获你的心。

在 API 世界中，了解它们独特的个性和特征是建立成功关系的关键。明智选择，你将打造出一套推动生产力和创新同时始终将用户体验放在首位的强大工具组合。

## Developing an Apex REST endpoint

标准 API 提供了广泛的功能并满足许多常见用例。然而，有时这些标准方案无法满足 ISV 对灵活性的需求或不符合其特定要求。这就需要构建自定义 Apex REST 端点。

Apex REST 方法使开发人员能够使用 Apex 编程语言构建自定义 RESTful Web 服务。这些服务在外部调用时，填补了标准 Salesforce API 可能不足的能力鸿沟。方法提供了灵活性，使开发人员能够设计符合特定需求的 API。此外，Apex REST 方法与标准 HTTP 方法无缝集成，便于执行 CRUD 操作和各种其他操作。

要使一个 Apex 类成为外部服务的灯塔，它需要被 **global** 注解装饰。这不仅将该类公开为 RESTful Web 服务，还确保其可从 Salesforce 域外部访问。

## Best practices for designing your APIs

API 设计是一个细致的过程，遵循行业最佳实践可确保其稳健、可理解且安全：

- Consistency：命名约定以及请求和响应结构的一致性可增强可用性。
- Versioning：在初始阶段进行版本控制可促进变更管理，防止潜在中断。
- HTTP Status Codes：使用合适的 HTTP 状态码描述 API 调用的结果至关重要。
- Error Handling：清晰且有意义的错误消息有助于用户进行故障排除。
- Security：纳入强大的身份验证和授权层。在 Salesforce 上下文中，OAuth 通常是首选。此外，建立输入清理和验证以防止注入和 XSS 攻击至关重要。我们将在下一章更详细地探讨这些安全要素。
- Pagination：为数据密集型端点实现分页，将结果分割为易于消化的部分。
- Rate Limiting：执行速率限制，确保公平使用并减轻潜在滥用。
- Documentation：详细的文档，涵盖请求-响应结构、错误和示例等方面，可提升用户体验。

## Potential pitfalls

在 API 开发和部署的迷宫中导航时，某些陷阱会挑战旅程：

- Versioning Issues：明显缺少版本控制会引发重大挑战，尤其是在产品演进且 REST 端点修改不可避免时。当现有用户绑定到旧版本时，可能导致创建近乎相同的 REST 端点。一种补救措施是在 urlMapping 中嵌入版本号，例如 `@RestResource(urlMapping='/AccountsList/v1/*')`。从初始版本起就采用此模式可避免未来的复杂问题。
- Information Disclosure Vulnerability：诸如披露系统或调试数据之类的细微泄漏可为对手提供洞察，可能促进攻击策略。此类泄漏通常发生在系统或调试数据意外通过输出流或日志机制渗出时。为防范此问题，请创建自定义错误消息，细心地掩盖诸如堆栈跟踪、路径信息或任何调试数据等复杂细节。必须在所有 API 中修复此类漏洞。此漏洞可能是导致安全审查失败的原因，我们将在下一章深入探讨这一主题。

## API rate limit management – ensuring efficient usage

API 速率限制是 Salesforce 管理其资源需求的机制。它在 24 小时内设置了应用程序可进行的 API 调用次数的最大限制。此功能旨在
