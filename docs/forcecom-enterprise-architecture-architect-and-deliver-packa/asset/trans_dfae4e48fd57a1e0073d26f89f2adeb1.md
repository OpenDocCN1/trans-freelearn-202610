

- 3. 上传包的发布版本，并在测试开发组织中安装它。

- 4. 打开开发者控制台，创建一个名为 ApplicationAPITest 的 Apex 类。

如果在设置菜单的 Apex 类页面中检查测试开发组织中的 `ComplianceService` 类，您可以看到刚刚公开的新 API 方法和类型现在已可见（不显示代码）。这为开发者 X 了解您的 API 提供了一种基本方式，此外还有您提供的任何文档。`ComplianceService` 的部分外观在以下屏幕截图中显示。

您还可以看到一个版本下拉框，它允许开发者 X 浏览 API 定义的不同版本。请注意，显示的版本可能与您的不同，具体取决于到目前为止在本书中上传了多少次包：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_363_0.png)

在订户组织的类列表中，所有 `global` Apex 类旁边也会显示安全链接。这样，订户组织管理员就可以通过配置文件控制 API 执行的权限。请注意，这仅适用于开发者 X 调用的全局控制器方法、自定义或扩展控制器，或 Web Service 调用。您还可以通过适当的权限集（如前一章所述）授予访问权限。这意味着，如果用户被分配了授予其访问应用程序特定功能集的权限集，那么通过 API 访问这些功能也包括在内。

如果要单独控制 API 权限，请创建功能范围独立的权限集：

| 操作 | 名称 | 命名空间前缀 |
| :--- | :--- | :--- |
| 编辑 | Security Application | fforce |
| 编辑 | Cars | fforce |
| 编辑 | CarsSelector | fforce |
| 编辑 | ComplianceController | fforce |
| 编辑 | Security ComplianceService | fforce |

## 从 Apex 调用应用程序 API

本节使用的代码采用 Apex 测试方法（未包含在 FormulaForce 应用程序中）。作为运行 API 代码示例的简便方法，我们将使用 Apex 测试来说明本章包含的示例代码中 Apex API 的用法。我们将查看的两个测试方法会在调用季节 API 创建一些测试数据（当然，这些数据仅在测试执行范围内的数据库中存在）后调用合规 API。

在以下示例中，由 SeasonService.createTestSeason API 创建的 Driver 记录未指明驾驶员持有 FIA 超级驾照。因此，预期 ComplianceService.verify 方法会抛出一个报告此问题的异常。以下示例测试代码仅为了本章说明目的而断言此行为：

```
@IsTest
private class ApplicationAPITest {
    @IsTest
    private static void demoComplianceVerifyAPI() {
        // Create some test data via the SeasonService
        fforce.SeasonService.createTestSeason();

        // Query Drivers to test compliance against
        Map<Id, fforce__Driver__c> driversById =
            new Map<Id, fforce__Driver__c>(
            [select Id from fforce__Driver__c]);

        try {
            // Invoke the compliance engine for the drivers
            fforce.ComplianceService.verify(
                driversById.keySet());
            System.assert(false, 'Expected failures');
        }
        catch (fforce.ComplianceService.ComplianceException e) {
// Check the results
System.assertEquals(
    '4.1', e.failures[0].complianceCode);
System.assertEquals(
    'Driver must have a FIA Super License.',
    e.failures[0].failureReason);
}
}
}
```

本示例中使用的命名空间是 fforce。您的命名空间会有所不同。替换命名空间以使代码示例能够编译（您也可以在本章的示例代码中找到此类）。

运行测试以确认 API 是否按预期工作。

由于您使用了应用程序其余部分所用的相同 Service 层方法，您围绕此类的现有单元测试和集成测试应该足以让您对此作为应用程序 API 的健壮性充满信心。如果不是，改进围绕 Service 层的测试将同样有利于外部和内部调用者。

请注意，当您在 ApplicationAPITest 类中单击显示依赖项按钮时，它会显示从包中引用的 API 和自定义对象：

显示依赖项

Apex 类 ApplicationAPITest 的依赖项信息

<< 返回 Apex 类: ApplicationAPITest

当一个组件引用另一个组件、权限或下方项目时，就会创建依赖关系。此外，对象级操作范围（即数据操作）也已列出。要查看操作范围的字段级详细信息，请单击对象名称旁边的字段。

对象操作范围

Driver (已安装包: FormulaForce) [字段]

Apex 类、触发器和页面引用

ComplianceService (已安装包: FormulaForce)

SeasonService (已安装包: FormulaForce)

## 修改和弃用应用程序 API

当一个 `global` 类方法或成员通过包含在已发布的包中而发布后，将无法将其删除或更改。例如，尝试将 `ComplianceService.verify` 方法重命名为 `verification`（例如）。当您尝试保存类时，将收到以下错误：

```
(ComplianceService) Global/WebService identifiers cannot be removed from managed application: Method: void verify(SET<Id>) (Line: 1)
```

> 如果您想在保留更改新 API 的能力的同时进行一些测试或分发您的包，请将您的包作为 Beta 上传，直到您对 API 定义满意为止。请注意，您当然可以随时更改 API 的行为（由其背后的 Apex 代码决定），但这必须谨慎进行。

`@Deprecated` 注解可用于有效地从后续包版本消费者的角度删除应用了 `global` 的类、接口、方法或成员的可见性。`Developer X` 仍可以通过类版本设置（在 **版本设置** 选项卡中显示，如以下各节中详细所述）引用包的早期版本来访问已弃用的项目。

> 在编写本文时，`@Deprecated` 标志无法在未托管的开发者组织中使用。在后续章节中，我们将讨论一种团队开发模型，该模型允许通过在源代码控制系统中管理代码并为每位开发者单独管理开发者组织来开发您的应用程序。尽管这种开发风格有许多好处，但它排除了使用 `@Deprecated` 标志，因为此类组织实际上是未托管的。

## 对 Apex API 定义进行版本控制

保存时，`ApplicationAPITest` 类、**版本设置** 选项卡和 **依赖项** 页面会反映类中的代码正在引用 FormulaForce 包中的 `ComplianceService` 和 `SeasonService` 类。

在以下屏幕截图中，版本显示为 v1.8，因为这是首次创建 Apex 类时包的当前版本（在您的情况中可能不同，具体取决于您迄今上传了多少次包）：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_367_0.png)

在以下步骤中，我们将发布包含新合规 API 的新版本包，以探索平台如何提供 Apex API 版本控制：

- 1. 在打包组织中，取消注释 `ComplianceService.report` 方法。此方法的工作方式与 verify 方法类似，但不会抛出异常；相反，它将返回包含合规通过和失败的完整结果列表。
- 2. 上传包的新发布版本并将其安装到测试开发组织中。
- 3. 在测试开发组织中，打开 Apex 类 `ApplicationAPITest`。

尝试添加以下新测试方法并保存类：

```
@IsTest
private static void demoComplianceReportAPI() {
    // Create some test data via the SeasonService
    fforce.SeasonService.createTestSeason();

    // Query Drivers to test compliance against
    Map<Id, fforce__Driver__c> driversById = new Map<Id, fforce__Driver__c>(
        [select Id from fforce__Driver__c]);

    // Update the licenses for the drivers
    for(fforce__Driver__c driver : driversById.values())
```

