
can’t restrict subscribers from providing translations, this feature allows them to tailor your app to their specific context and needs.

## Apex – 可定制 Salesforce 应用的支柱

Apex 是一种功能强大、强类型的编程语言，用于在 Salesforce 平台上开发自定义业务逻辑。它旨在执行服务器端操作，使开发人员能够创建动态且多功能的 Salesforce 应用，以满足特定的业务需求。

AppExchange 合作伙伴面临的挑战之一是创建能够满足客户独特需求的应用。作为受管包，Salesforce 应用的核心在安装后无法由订阅者修改。然而，最终客户可能有不同的需求，从而需要定制和可扩展性。Forbes 报道称，71% 的客户对应用缺乏定制选项感到沮丧。

为解决这一复杂性，Salesforce 平台即使在受管包中也提供了大量的定制机会。最终客户可以通过 Salesforce 平台广泛调整合作伙伴应用以符合其业务需求。他们可以利用自定义字段、流程、Apex 和其他工具来个性化应用。此外，Dynamic Apex 使开发人员能够创建具有固定核心但提供额外定制空间的应用，从而提升最终用户的体验并满足其特定需求。

## Dynamic Apex – AppExchange 应用中适应性的力量

Dynamic Apex 是 Apex 编程的一个关键方面，为 Salesforce 开发人员提供了一套工具，用于构建高度可定制和可适应的应用。它包括 **动态 Salesforce 对象查询语言** (**动态 SOQL**)、**动态 Salesforce 对象搜索语言** (**动态 SOSL**)，以及 **动态数据操作语言** (**动态 DML**)，所有这些都是解决 AppExchange 开发中独特业务需求和复杂性的关键。

### What is Dynamic Apex?

Apex 是 Salesforce 的专有强类型编程语言，主要用于构建自定义业务逻辑。传统的 Apex 提供了用于开发应用的强大功能集，但通常不支持基于动态条件的运行时更改。Dynamic Apex 正填补了这一空白。

从本质上讲，Dynamic Apex¹ 允许开发人员构建能够在运行时引用 SOQL、SOSL 和 DML 字符串的应用，从而实现可适应的查询、搜索和数据操作的创建。

1 https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_dynamic.htm

The components of Dynamic Apex are set out here:

- **动态 SOQL**：动态 SOQL 允许您使用 Apex 代码在运行时构建 SOQL 字符串。这允许根据特定的运行时条件或参数创建复杂、灵活的查询，从而使应用能够以多变和适应性强的方式与数据交互。
- **动态 SOSL：** 与动态 SOQL 类似，动态 SOSL 允许您使用 Apex 代码在运行时创建 SOSL 搜索字符串。此功能有助于根据用户输入或其他动态参数创建搜索策略，从而实现更定制化、更强大的搜索功能。
- **动态 DML：** 动态 DML 操作允许开发人员动态构建和执行数据操作任务。这可以包括 insert、update、delete 和 upsert 操作。因此，动态 DML 支持 Salesforce 应用中的高度自适应数据处理。

### The importance of Dynamic Apex in AppExchange development

为 Salesforce AppExchange 市场开发应用通常需要高度的定制以满足不同的客户需求。在此，Dynamic Apex 的固有灵活性和适应性具有极大价值。

Dynamic Apex 可帮助您构建应用，使其根据客户组织的具体配置支持各种 Salesforce 功能。例如，一些组织可能启用了 Person Accounts，而其他组织可能未启用。基于 Dynamic Apex 构建的应用可以动态检查 Person Accounts 的存在情况并相应调整其功能。

这种灵活性不仅限于标准的 Salesforce 功能。Dynamic Apex 为应用与订阅者的本地自定义对象和字段无缝集成打开了大门。这意味着，与其受预定义配置限制，您的产品现在可以构建为与组织在受管包之上构建的独特定制和谐协作。

同样，像 Advanced Currency Management 这样的功能，可能在某些组织中启用而在其他组织中未启用，可以使用 Dynamic Apex 动态支持。应用可以检测组织中此类功能的状态并修改其操作以与可用功能保持一致。

### Dynamic SOQL example

LookupController 类是 Formula Debugger 受管包的一个组件，体现了 Dynamic SOQL 的多功能性。此控制器使用户能够使用动态查询从标准 Salesforce 对象和本地自定义对象中搜索和检索记录。您可以在此处看到其使用示例：

```
public with sharing class LookupController {
    public static List<sObject> search(String searchTerm, string searchObject) {
        String searchCondition = '%' +
String.escapeSingleQuotes(searchTerm) + '%';
        String searchQuery = 'SELECT Id, Name FROM ';
        searchQuery+=String.escapeSingleQuotes(searchObject);
        searchQuery+=' WHERE Name LIKE :searchCondition ';
        searchQuery+='WITH SECURITY_ENFORCED ORDER BY Name LIMIT 5';
        return Database.query(searchQuery);
    }
}
```

必须注意的是，虽然使用 Dynamic SOQL 增加了灵活性，但也引入了潜在的漏洞，尤其是 SOQL 注入。此攻击允许恶意行为者将其 SOQL 代码“注入”到查询中。为防止这种情况，LookupController 类使用 `String.escapeSingleQuotes` 方法确保用户输入得到清理，转义单引号并防止潜在的 SOQL 注入威胁。您将在 *第 6 章* 中了解更多关于此漏洞的信息。

## Dynamic Apex or extension package?

Salesforce ISV 开发人员经常面临将特定功能集成到并非所有客户组织中都启用的挑战。在这些情况下，一个重要的决策是是否在核心包中利用 Dynamic Apex，还是开发额外的扩展包。这两种方法各有其独特的优点和缺点，选择取决于各种因素，例如具体用例、可扩展性预期和应用复杂性。

### The need for this decision

让我们以同时支持 Person Accounts 和 Business Accounts 为例。Person Accounts 是主要用于 B2C 场景的特定 Salesforce 功能，而 Business Accounts 适用于 B2B 关系。并非所有 Salesforce 组织都启用 Person Accounts 功能。

如果您的解决方案需要同时满足这两种账户类型，一个关键挑战是避免在 Person Accounts 上创建包依赖，因为这会限制包的兼容性。因此，您必须开发一个无论 Person Accounts 功能是否启用都能有效运行的包。这就是在 Dynamic Apex 和创建扩展包之间做出决策变得至关重要的时刻。

Let’s look at the pros and cons of each option in a tabular format, as follows:

| Method | Advantages | Disadvantages |
|---|---|---|
| Extension packages | Clear segregation of business logic<br>Facilitates standard SOQL queries<br>Allows incremental updates specific to the added feature<br>A single security review can include one base | Additional security reviews are required if more than four extension packages are created<br>Extra security reviews may be required for extension packages developed after the initial review of the base package<br>Increased management effort due to separate packages |
