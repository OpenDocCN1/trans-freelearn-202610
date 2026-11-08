
## Execute Prpt Component

Execute Prpt Component 与 PRPT 组件类似，不同之处在于预览在模态窗口中打开，并且可以通过按钮访问。以下截图描述了组件的设置以及在下一个示例中使用的配置：

![](img/9337ac94537bf7e092e1ea6531dce8fd_656_0.png)

## Schedule PRPT Component

`Schedule PRPT component` 使您能够使用布局层中的按钮安排 `.prpt` 格式的 Pentaho 报表。按下按钮后，将显示一个模态窗口，其中包含本章前面介绍的相同设置。以下截图描述了组件的设置以及在下一个示例中使用的配置：

![](img/9337ac94537bf7e092e1ea6531dce8fd_657_0.png)

## Example of CDE dashboard

现在您已经了解 Pentaho 仪表板是什么以及其开发的基础知识，让我们看一个具体的示例，定义 `my_first_report` 报表的一个复杂预览。如果您按照本章前面描述的示例进行操作，您应该已经在 Pentaho 仓库的 `/public` 路径下拥有一个可用的 `my_first_report.prpt` 文件。如果没有，您可以在 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 仓库中找到它，具体在 `Chapter 15 - Using reports in Pentaho Business Analytics Platform/my_reports` 文件夹中。

本节中我们将构建的示例仪表板将由一个左侧面板组成，使用页面大部分宽度，使用 `PRPT Component` 预览报表，以及一个右侧面板，包含两个按钮：连接到 `Schedule PRPT Component` 的 Schedule 按钮和连接到 `Execute Prpt Component` 的 Launch 按钮。

一旦您使用 Pentaho 用户控制台中的 File | New | CDE Dashboard 项创建了一个新的 CDE 仪表板，请在布局透视图中按以下截图所示设置行和列：

![](img/9337ac94537bf7e092e1ea6531dce8fd_659_0.png)

除了行和列的嵌套结构外，其他内容其实并不重要，但建议用正确的值填充 `Name` 属性（`obj1`、`obj2` 和 `obj3`），因为它将被用作组件与布局之间的链接。

完成后，单击组件透视图并双击右侧列表中的三个组件中的每一个（`PRPT Component`、`Schedule PRPT Component` 和 `Execute Prpt Component`）。此操作将自动把这三个组件添加到包含报表组件的中央面板中。

作为报表组件可用后，按前一张截图所述配置它们。需要设置且相关的必填属性如下：

- Name：这适用于全部三个组件，填写一个随机但有意义的字符串。
- Path：这适用于全部三个组件，填写 `/public/my_first_report.prpt` 字符串。此属性将组件指向仓库中存储的正确 Pentaho 报表。
- Label：这适用于两个按钮（`Schedule PRPT Component` 和 `Execute PRPT Component`）。此属性定义为每个按钮显示的内容。
- HtmlObj：这适用于全部三个组件。此属性是必填且相关的，因为它链接到布局层，具体到组件将显示的布局元素。

完成此任务后，可以使用顶部菜单中的 Save 项保存 CDE 仪表板。单击后，模态窗口会要求您输入 Pentaho 仓库的路径和名称。在此示例中，将使用 `my first dashboard` 名称。从那时起，Pentaho 仓库中会出现两个新文件。选择带有 CDE 蓝色图标的文件并单击右侧面板中的 Open 项，将显示类似以下截图的预览：

![](img/9337ac94537bf7e092e1ea6531dce8fd_660_0.png)

在 CDE 仪表板中，您应该能识别出开发的结构：左侧有一个大列，包含使用 `PRPT` Component 的预览，右侧有一个较小的列，包含三个较小的子行：第一个包含 Schedule 按钮，第二个仅为间隔，第三个包含 Launch 按钮。

*如果您想尝试 CDE 组件的所有属性*

![](img/9337ac94537bf7e092e1ea6531dce8fd_661_0.png)

以便发现它们的行为，我们强烈建议您花时间去做。正如 Pentaho Report Designer 所建议的那样，对于 CDE 仪表板也是如此，最好的老师是通过一些示例进行实验。不要担心做错什么。您不会对 Pentaho Business Analytics Platform 造成任何不良影响，只能学习。

要完成示例，在以下截图中您可以看到单击 Launch 按钮后浏览器的外观：

![](img/9337ac94537bf7e092e1ea6531dce8fd_661_1.png)

在以下截图中，您可以看到单击 Schedule 按钮时浏览器的外观：

![](img/9337ac94537bf7e092e1ea6531dce8fd_662_0.png)

作为本节要分享的最后一个主题，让我们看看如何使用公共 URL 访问 CDE 仪表板。正如您在 Pentaho 报表中所看到的，CDE 仪表板的公共 URL 定义如下：

- 基础 URL。在我们的示例中，http://localhost:8080/pentaho。
- 端点路径。在我们的示例中，/api/repos/。
- Pentaho 仓库中 .prpt 文件的路径，其中路径分隔符为分号 (:)。该字符串在 URL 中进行 URL 编码。在我们的示例中，%3Apublic%3A。
- CDE 仪表板的 URL 编码名称。在我们的示例中，my%20first%20dashboard 后跟 .wcdf 扩展名。
- 端点后缀。在我们的示例中，/generatedContent。

在以下 URL 中，您可以找到本章示例的链接：

```
http://localhost:8080/pentaho/api/repos/%3Apublic%3Amy%20first%20dashboard.wcdf/
```

*如果您在开发示例时遇到任何问题或太懒得开发它，在 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 仓库中，具体在 `Chapter 15 - Using reports in Pentaho Business Analytics Platform/my_dashboards` 文件夹中，您*

![](img/9337ac94537bf7e092e1ea6531dce8fd_662_1.png)

![](img/9337ac94537bf7e092e1ea6531dce8fd_663_0.png)

可以找到此处介绍的 CDE 仪表板。要将其安装到 Pentaho Business Analytics Platform，只需将两个文件（一个带有 `.cdfde` 扩展名，另一个带有 `.wcdf` 扩展名）上传到 Pentaho 仓库的 `/public` 文件夹即可。

## Summary

在本章中，您学习了如何在 Pentaho Business Analytics Platform 中使用现有的 Pentaho 报表，该平台是供最终用户和开发人员使用的前端 Web 应用程序。本章介绍了 Pentaho Business Analytics Platform 的基础知识，并提供了如何安装它以及上传 Pentaho 报表的说明。之后，您学习了如何预览它、如何管理权限、如何安排它以及如何使用 Pentaho CTools 开发 Pentaho 仪表板。

现在您已经阅读了本章，您应该对 Pentaho Business Analytics Platform 以及管理 Pentaho 报表的所有基本功能感到熟悉。这对于开发人员和信息技术人员来说是一项高级任务，用于管理最终客户和经理的前端。

在下一章中，您将了解更多关于在 ETL 作业中使用 Pentaho 报表的信息（https://en.wikipedia.org/wiki/Extract,_transform,_load），使用 Pentaho Data Integration（也称为 Kettle）。

## Using Reports in Pentaho Data Integration

在第 15 章 *Using Reports in Pentaho Business Analytics Platform* 中介绍了 Pentaho Business Analytics Platform 后，在本章中您将学习如何在 Pentaho Data Integration（也称为 Kettle）中使用 Pentaho 报表。这将提供强大的 ETL 功能。

在本章中，您将了解 Pentaho Data Integration 和 ETL 工具的一般基础知识，并提供如何安装它们以及将 Pentaho 报表用作输出任务的说明。像往常一样，本章中您将看到一个实际运行的示例。目标始终相同：通过示例学习。

本章作为开发人员和信息技术人员的教程编写。最好的学习方式是在阅读时在笔记本电脑上按照说明操作。到本章结束时，您将对使用 Pentaho Data Integration（也称为 Kettle）与 Pentaho 报表的基础知识感到熟悉。这对于您的未来项目将非常有用，使您能够使用功能齐全的 ETL 工具。
