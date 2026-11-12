

使用 Salesforce 提供的组件并非完整解决方案；例如，除非开发者启用，否则 Chatter 不会自动出现在页面上。另一个重要注意事项是，添加到您对象中的自定义字段或相关列表（由订阅者添加的）在页面代码中需要特别考虑，以确保这些内容得以显示，例如通过使用字段集和 apex:relatedList 组件。

最后，请注意，虽然自定义对象操作覆盖（由您定义）可以打包，但它们不可升级，因此更改不会应用于现有订阅者。另外，如果订阅者希望通过标准 UI 访问自定义对象，他们可以在安装后移除这些覆盖，因此这是另一个理由，确保您将所有对象的标准 UI 视为标准实践。

作为覆盖现有标准操作的替代方案，请考虑创建自定义按钮，将其放置在对象的详细信息页面和/或列表视图布局上，并将按钮命名为类似“增强编辑”或“增强新建”的名称。自定义按钮在 Salesforce Classic 和 LEX 标准 UI 中均可用，用于调用 Visualforce 页面。但是，它们不支持您构建的基于 Lightning 的自定义 UI。要从 LEX 中的标准 UI 调用 Lightning 组件，您可以将其嵌入到 Lightning 记录页面布局中（如第 6 章“应用程序域层”所示），或使用 Lightning 组件操作（在第 9 章“Lightning”中讨论）。

## 将标准 UI 与自定义 UI 相结合

除非您的用户体验与标准 UI 提供的体验完全不同，否则请始终尽可能保留标准 UI。平台提供了多种方法来实现这一点，我们将在本章和下一章中探讨这些方法。在本节中，我们将探讨在标准 UI 的特定区域嵌入自定义 UI 部分以及反之亦然的方法。

## 在标准 UI 中嵌入自定义 UI

在第 6 章“应用程序域层”中，我们研究了一个可以放置在 Lightning 记录页面布局上的 Lightning 组件。此方法仅适用于 LEX，并且还提供一些额外优势，例如能够监听用户内联编辑记录详细信息时 LEX 触发的事件。此方法目前仅适用于查看记录详细信息的情况。

以下截图提醒您，**合规性验证**组件放置在 LEX 标准 UI 上时的外观：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_279_0.png)

现在让我们看看如何自定义同时适用于 Salesforce Classic 和 LEX 的标准 UI 的示例。通过在 Race 对象的详细信息页面布局中嵌入 Visualforce 页面（如以下截图所示）来实现。页面布局在 Salesforce Classic 和 LEX 中均可用。

以下 Visualforce 页面使用 `$User.UIThemeDisplayed` **全局变量**来相应地设置页面样式。它重复使用了上一章中的**扩展控制器**：

```apex
<apex:page standardController="Race__c" extensions="RaceSummaryController"
 action="{!loadSummary}">
<apex:slds/>
<apex:outputPanel
 rendered="{!$User.UIThemeDisplayed == 'Theme3'}">
<apex:pageBlock>
<apex:pageBlockTable value="{!Summary}" var="summaryRow">
<apex:column value="{!summaryRow.Position}">
<apex:facet name="header">Position</apex:facet>
</apex:column>
<apex:column value="{!summaryRow.Driver}">
<apex:facet name="header">Driver</apex:facet>
</apex:column>
<apex:column value="{!summaryRow.Team}">
<apex:facet name="header">Team</apex:facet>
</apex:column>
<apex:column value="{!summaryRow.Car}" >
<apex:facet name="header">Car</apex:facet>
</apex:column>
</apex:pageBlockTable>
</apex:pageBlock>
</apex:outputPanel>
<apex:outputPanel
rendered="{!$User.UIThemeDisplayed == 'Theme4d'}" >
<div class="slds">
<table
class="slds-table slds-table--bordered
slds-table--cell-buffer">
<thead>
<tr class="slds-text-title--caps">
<th scope="col">
<div
class="slds-truncate"title="Position">Position</div>
</th>
<th scope="col">
<div class="slds-truncate"title="Driver">Driver</div>
</th>
<th scope="col">
<div class="slds-truncate"title="Team">Team</div>
</th>
<th scope="col">
<div class="slds-truncate"title="Car">Car</div>
</th>
</tr>
</thead>
<tbody>
<apex:repeat value="{!Summary}" var="summaryRow" >
<tr>
<th scope="row"data-label="Position" >
<div
class="slds-truncate">{!summaryRow.Position}</div>
</th>
<td data-label="Driver" >
<div class="slds-truncate">{!summaryRow.Driver}</div>
</td>
<td data-label="Team" >
<div class="slds-truncate">{!summaryRow.Team}</div>
</td>
<td data-label="Car" >
<div class="slds-truncate">{!summaryRow.Car}</div>
</td>
</tr>
</apex:repeat>
</tbody>
</table>
</div>
</apex:outputPanel>
</apex:page>
```

由于该页面使用 StandardController，它会出现在布局编辑器中，并可以放置在布局上：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_281_0.png)

然后，您可以将其拖放到布局上的现有或新部分中。结果在 Salesforce Classic 中将类似于以下截图：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_281_1.png)

在 Salesforce LEX 中，页面如下截图所示：

![](img/bd6ecd0ea6a28b6cca2aa631a00b3793_282_0.png)

在 Salesforce Classic 中，此方法仅适用于用户查看记录时的布局。但是，您可以通过此类 Visualforce 页面为用户提供编辑信息的途径，即使它们呈现在记录的查看页面上。但是，这样做时无法刷新主页面。虽然 Visualforce 页面的宽度会自动调整，但高度是固定的，因此请务必根据常见数据量进行相应设置。与 Salesforce Classic 不同，Salesforce LEX 中的用户在创建和编辑记录以及查看记录时，都会在布局中看到嵌入的 Visualforce 页面。请确保您的页面在 LEX 中预期这些场景，例如，在记录创建场景中，StandardController.getId 方法将返回 null，您的代码应相应地处理此情况。

## 在自定义 UI 中嵌入标准 UI

根据您使用的自定义 UI 技术，有不同的选项可以嵌入标准 UI 页面布局。请注意，执行此操作时，任何嵌入的 Visualforce 页面也会一同包含。

对于 Salesforce Classic，可以在 Visualforce 页面上使用 `apex:detail` 标签来呈现标准 UI 的大部分外观和感觉；请注意，当页面在 LEX 中显示时，它不会自动重新设置样式。该组件的粒度不够细；例如，您无法在其呈现的标准按钮旁边添加自己的按钮。这意味着您的页面按钮需要在页面的其他位置呈现，从而使 UI 看起来不一致且令人困惑。此外，该组件仅呈现记录布局的只读视图。在大多数情况下，本章前面描述的混合方法为大多数此类需求提供了更好的解决方案。

对于 Salesforce LEX，`force:recordEdit` 和 `force:recordView` 组件支持在使用 Lightning 组件构建的自定义 UI 中进行记录编辑和查看。与 Visualforce 中的 `apex:detail` 组件不同，不会呈现操作按钮。开发者负责通过触发 `recordSave` **Lightning 事件**来通知组件何时保存详细信息。当记录保存成功时，组件会响应 `onSaveSuccess` 事件。

## 扩展 Salesforce 标准 UI

前面的部分描述了一种在标准 UI 内嵌入自定义 UI 的方法，以便使用您自己的应用程序功能扩展 Salesforce UI。以下部分分别概述了 Salesforce Classic 和 LEX 中可用的进一步选项。某些功能在两者中都存在，而其他功能则在 LEX 中通过从不同区域嵌入和调用 Lightning 组件的能力进行了重新构思。

## Visualforce 页面

Visualforce 不仅是为您的应用程序创建全新页面的强大方式，而且还在多个位置增强了 Salesforce 用户界面。如我们迄今所见，您可以使用它来扩展标准 UI 布局和自定义按钮。以下是接受 Visualforce 页面的平台区域列表：

- 布局部分
- 自定义按钮
- 自定义选项卡
- 自定义选项卡闪屏页面
- Chatter 自定义发布操作
- 侧边栏组件
- 仪表板组件
- Force.com 网站

