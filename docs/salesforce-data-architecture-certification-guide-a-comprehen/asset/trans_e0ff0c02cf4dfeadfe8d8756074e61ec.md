

## 练习题
- 1. 在B2C场景中，需要访问报表时，需要哪种Experience Cloud许可类型？
- 2. 哪种Experience Cloud许可类型会暴露**报价**对象？
- 3. 访问知识库需要哪种Experience Cloud许可类型？
- 4. 哪种Experience Cloud许可类型仅通过共享集促进记录共享？
- 5. 哪种Experience Cloud许可类型允许创建仪表板？
- 6. 哪些Experience Cloud许可类型允许使用共享规则？
- 7. 哪种Experience Cloud许可类型允许创建报表？
- 8. 哪种Experience Cloud许可类型会暴露**潜在客户**和**商机**对象？
- 9. 哪种技术用于以**开放数据协议（OData）** 2.0或OData 4.0格式展示外部数据？
- 10. 哪种技术用于在Salesforce平台上处理和执行跨整个数据集的长时运行Apex操作？
- 11. 哪种技术用于在Salesforce平台上读取、上传或删除大型数据集？
- 12. 哪种技术用于将一个Salesforce实例中的数据展示给另一个Salesforce实例？
- 13. Customer 360是Salesforce提供的一种解决方案，旨在提供什么？
- 14. Marketing Cloud和Salesforce使用什么连接？
- 15. 在Customer 360中，推荐使用哪种技术将外部数据源的数据整合到Salesforce平台上？

## 答案
- 1. Customer Community Plus
- 2. Partner Community
- 3. Customer Community
- 4. Customer Community
- 5. Partner Community
- 6. Partner Community和Customer Community Plus
- 7. Customer Community Plus
- 8. Partner Community
- 9. Salesforce Connect
- 10. Batch Apex
- 11. Bulk API
- 12. Salesforce to Salesforce
- 13. 客户的360度视图
- 14. 适用于Salesforce平台的Marketing Cloud连接器
- 15. MuleSoft

## 延伸阅读

Salesforce Experience Cloud许可：

```
https://help.salesforce.com/s/articleView?id=users_license_types_communities.htm&type=5&language=en_US
```

设置共享集：

```
https://help.salesforce.com/s/articleView?id=sf.networks_setting_light_users.htm&type=5
```

Salesforce数据存储分配：

```
https://help.salesforce.com/s/articleView?id=sf.overview_storage.htm&type=5
```

CDP：

```
https://www.salesforce.com/ap/solutions/customer-360/
```

Salesforce to Salesforce：

```
https://help.salesforce.com/s/articleView?id=sf.business_network_intro.htm&type=5
```

Bulk API 2.0简介：

```
https://developer.salesforce.com/docs/atlas.en-us.api_asynch.meta/api_asynch/asynch_api_intro.htm
```

## 数据治理

保护**Salesforce**平台上的数据是Salesforce数据架构师角色的关键部分。本章涵盖*构建合规解决方案*背后的概念，以及Salesforce平台如何提供有助于企业**数据治理**战略技术基础的功能。

在本章中，我们将讨论以下主题：

- 实施合规数据模型的注意事项
- 企业数据治理

虽然有些人可能认为数据治理是一个有点*枯燥*或无聊的话题，但请继续阅读，你会发现为什么在设计*可扩展、安全*的Salesforce平台解决方案时它很重要。数据治理占认证大纲的10%，因此在参加考试时可以获得一些宝贵的分数。

本章是数据管理、安全性和治理方面相互交织的几个主题的集大成者。在本章结束时，你将理解合规数据模型背后的概念和法规，例如**通用数据保护条例（GDPR）**、**个人身份信息（PII）**等，并且能够阐明企业数据治理是什么以及它与Salesforce平台的关系。

让我们从理解实施合规数据模型的注意事项开始。

## 实施合规数据模型的注意事项

Salesforce认证数据架构师认证大纲在数据治理的背景下讨论了实施**合规**数据模型。作为Salesforce数据架构师，我们有责任确保能够推荐一种设计数据模型的方法，使其符合以下等法规中规定的**合规标准**：

- **通用数据保护条例（GDPR）**，欧盟
- **加州消费者隐私法案（CCPA）**，美国
- **个人信息保护法（PIPA）**，日本
- **通用数据保护法（LGPD）**，巴西
- **个人信息保护和电子文件法案（PIPEDA）**，加拿大

这包括审查可用于识别、分类和保护敏感数据的各种选项。

我们现在来看这样一项法规，即GDPR。

### 什么是GDPR？

GDPR是欧洲数据保护法，规定了如何处理欧盟个人的个人信息。

GDPR旨在控制组织或政府如何使用信息。个人有权知道他们的信息是如何被收集、处理和存储的。他们有权要求公司、政府或实体提供其持有的关于他们的信息，并有权被遗忘。如果提出这样的请求，持有数据的组织必须删除该数据，但须遵守公平数据处理原则（个人在自由选择加入收集此信息的公司或实体时必须已被告知该原则）。公司有法律义务公平地收集信息，仅将其用于所需的特定目的，安全地存储信息，并在不再需要时删除信息。如果公司违反GDPR，罚款可能高达2000万欧元或年度全球营业额的4%中的较高者（这是根据GDPR本身的规定）。

从软件应用程序的角度来看，遵守GDPR的影响包括确保应用程序数据模型能够记录同意和隐私偏好。正如我们已经知道的，Salesforce包含几个标准对象来帮助记录偏好和同意（例如Individual对象）。可以根据特定需求添加额外的自定义对象。

### Individual对象

根据https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_individual.htm上的Salesforce文档，`Individual`对象代表客户的数据隐私和保护偏好。基于Individual对象的数据隐私记录存储客户的偏好。数据隐私记录与相关的潜在客户、联系人、个人账户和用户相关联。

当启用`Individual`对象时，可以通过将`Individual`字段添加到**潜在客户**、**联系人**和**个人账户**页面布局来向用户公开。

> **重要说明**
> 启用`Individual`对象：如果在Salesforce中启用了**数据保护和隐私**，则`Individual`对象可用。这可以通过在**Salesforce设置**中的**数据保护和隐私**下选择**在记录中提供数据保护详细信息**来完成。

### *处理GDPR数据*

个人数据和敏感个人数据受GDPR保护，需要**识别**、**分类**和**保护**。Salesforce如何帮助解决数据保护问题？Salesforce包含多项功能来帮助保护数据免受未经授权的访问，包括以下内容：

- Classic和平台Shield加密
- 数据掩码
- 字段级安全
- 共享设置
- 事件监控
- 会话安全
- 字段历史跟踪

让我们详细讨论这些主题。

### *Classic加密*

包含在基础许可证中的**classic加密**使用128位**高级加密标准（AES）**为自定义字段（一种独特的自定义字段类型）的静态数据提供加密。用户需要在其配置文件或通过分配的权限集启用**查看加密数据**配置文件权限才能读取加密字段值。Classic加密有助于对没有权限读取值的用户掩码加密字段。他们将看到一系列星号。使用classic加密的加密字段无法搜索，也不能用于Flow、审批流程、工作流更新等自动化操作。因此，在加密某些仅以掩码方式存储数据、供不应查看记录的人查看的字段时，请考虑使用classic加密。如果字段需要静态加密、需要用户可搜索或可在Flow等自动化场景中读取，请考虑改用Shield加密。

