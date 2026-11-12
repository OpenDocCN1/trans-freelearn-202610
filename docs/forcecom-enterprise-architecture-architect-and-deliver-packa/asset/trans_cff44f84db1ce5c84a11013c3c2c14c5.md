

例如，为报告 API 进行版本控制时，我们需要在创建 API 类时将其类型作为内部 Apex 类捕获。以下示例说明了 v1.0 的此过程。您可以看到 API 的定义如何封装在由 v1_0 后缀表示的特定版本的 Apex 类中：

```
@RestResource(urlMapping='/v1.0/compliance')
global with sharing class ComplianceResource_v1_0 {

global class VerifyResult {
    global Id recordId;
    global String complianceCode;
    global Boolean passed;
    global String failureReason;
}

@HttpPost
global static List<VerifyResult> report(List<Id> Ids) {

    List<VerifyResult> results = new List<VerifyResult>();
    for(ComplianceService.VerifyResult result :
        ComplianceService.report(new Set<Id>(Ids)))
        results.add(makeVerifyResult(result));
    return results;
}

private static VerifyResult
makeVerifyResult(ComplianceService.VerifyResult verifyResult) {
    VerifyResult restVerifyResult = new VerifyResult();
    restVerifyResult.recordId = verifyResult.recordId;
    restVerifyResult.complianceCode = verifyResult.complianceCode;
    restVerifyResult.passed = verifyResult.passed;
    restVerifyResult.failureReason = verifyResult.failureReason;
    return restVerifyResult;
}
}
```

您使用的版本号可以是任何您喜欢的内容，但您可以考虑遵循您的软件包版本编号序列。

以这种方式对 Apex REST API 进行版本控制并不理想，因为必须通过代码手动对定义进行版本控制，并且在软件包的每个版本中管理该代码会带来额外开销。但是，如果不采用这种方法，始终会暴露软件包中 Apex REST API 的最新定义。这意味着在升级软件包后，调用者会收到他们未预期的响应中的新字段。也没有方法可以弃用 Apex REST API。但是，由于对 `global` 类成员的删除限制，开发人员在意外删除会破坏现有请求的字段时有一定的保护。

## Exposing Lightning Components

在前一章中，我们查看了 FormulaForce 软件包中包含的几个组件。这些组件已全部通过 `global` 访问级别公开，以供订阅者组织中的工具和开发人员代码使用。

当开发人员在其自身组件中使用这些组件时，可以通过包元数据中的设施来表达他们希望平台在代码与打包组件交互时假定的软件包版本。这允许对组件的定义和行为进行版本控制。如果没有此信息，则假定当前安装的软件包版本。

`System.requestVersion`、`{!Version}` 和 `cmp.getVersion()` 可用于确定组件在 Apex 或 JavaScript 中需要遵循的版本。

## Extending Process Builder and Visualflow

**Process Builder** 或 **Visualflow** 工具允许用户创建自定义流程和类似向导的用户体验。这两种工具都提供大量可操作的动作，这些动作基于用户定义的条件执行。

可用操作可以由安装在组织中的软件包扩展。这为您提供了另一种公开功能以构建自定义场景的方法。这种方法不需要用户编写任何代码。

The following code creates an **Invocable Method**, which is a `global` class and a single method with some specific annotations. These annotations are recognized by the tools to present a dynamic configuration UI to allow the user to pass information to and from the method you annotate:

```
global with sharing class UpdateStandingsAction {

    global class UpdateStandingsParameters {

        @InvocableVariable(
        Label='Season Id'
        Description='Season to update the standings for',
        Required=True)
        global Id seasonId,
        @InvocableVariable(
        Label='Issue News Letter'
        Description='Send the newsletter out
        to the teams and drivers'
        Required=False)
        global Boolean issueNewsLetter;
    }
}
```

```
private SeasonService.UpdateStandings makeUpdateStandings() {
    SeasonService.UpdateStandings updateStandings =
    new SeasonService.UpdateStandings();
    updateStandings.seasonId = this.seasonId;
    updateStandings.issueNewsLetter = this.issueNewsLetter;
    return updateStandings;
}
}
```

```
@InvocableMethod(
Label='Update the leaderboard of the season'
Description='Updates the standings and optionally
sends the newsletter.')
global static void
updateStandings(
List<UpdateStandingsParameters> parameters) {
    // Marshall parameters into service parameters
    List<SeasonService.UpdateStandings> updateStandings =
    new List<SeasonService.UpdateStandings>();
    for (UpdateStandingsParameters parameter : parameters) {
    updateStandings.add(parameter.makeUpdateStandings());
    }
    // Call the service
    SeasonService.updateStandings(updateStandings);
}
}
```

> 为将此方法的参数与 Service 类方法的变化隔离开来，使用了 UpdateStandings Apex 类。如果需要，这还允许方法向不同方向演变。

以下显示了上述 Invocable Method 在 Process Builder 中的外观：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_380_0.png)

当比赛结束时，此方法可例如用作上述订阅者定义的 Process Builder 流程的一部分。虽然您可以在 Apex Trigger 中执行此操作，但这可能涉及特定于您软件包订阅者的某些附加条件。因此，这是 Process Builder 的一个良好用例，并通过 Invocable Method 公开 SeasonService.updateStandings 方法。

> 有关 Invocable Methods 和上述属性的更多信息，请查阅 Apex 开发者指南。关于可传递的参数类型存在各种限制，例如参数必须为列表形式。这与 Service 层的批量化要求非常契合。

## Versioning Invocable Methods

Process Builder 或 VisualFlow 都不会记录引用可调用方法时使用的软件包版本。因此，无法执行行为版本控制，System.requestVersion 返回当前安装的软件包版本。Invocable Method 的定义由与 global 修饰符使用相关的约束控制，我们已在本章中讨论过。作为最佳实践，请避免向参数类添加新的必需成员（通过 @InvocableVariable）。

## Alignment with Force.com extensibility features

以下是一些平台功能，可帮助确保您的应用程序功能对 Developer X 或订阅者组织管理员开放以进行扩展：

- **Apex Triggers**：Developer X 可以针对您的应用程序的托管自定义对象编写自己的触发器。这些触发器将在打包的触发器之外执行。例如，这允许 Developer X 实现字段默认值或自定义验证。Salesforce 不保证打包的触发器会在订阅者组织中在 Developer X 触发器之前或之后执行。为了确保无论触发器执行顺序如何都验证所有更改，请在打包触发器的 after 阶段中执行验证逻辑。

- **Field Sets**：如前几章所述，请尽可能经常在 Visualforce 页面上利用此功能，以确保区域和表格可以使用订阅者添加的字段进行扩展。如果您正在开发移动 UI，这些功能也可以与 JavaScript 框架一起使用。

- **E-mail templates**：如果您正在编写代码来发送电子邮件，请考虑提供一种机制，允许使用电子邮件模板自定义文本。可以通过自定义设置使用其开发者名称引用这些模板。还可以在此处利用 Field Sets 来提供一种简便方式，以包含订阅者管理员添加的其他自定义字段。

- **Visualforce components**：请考虑全局创建一些 Visualforce 组件，这样 Developer X 可以更轻松地构建替代的自定义 Visualforce 页面，而无需重新实现应用程序页面的所有方面。此类组件可以构建为允许在电子邮件模板中使用动态内容。

- **Lightning components**：请考虑公开软件包中使用的 Lightning 组件，以供 Lightning App Builder 和 Lightning Community Builder 等工具使用，正如我们在前一章中探讨的那样。这可以为管理员和开发人员提供很大的灵活性，以构建和自定义其业务的独特用户体验。

- **Dynamic Apex type creation**：可以使用 `Type.forName` 方法创建 Apex 类型的实例，并从该实例在运行时创建该类型的实例。此功能可用于允许 Developer X 编写 Apex 代码，您的应用程序代码可以在指定时间动态调用该代码。此方法将在即将到来的章节中详细讨论。

