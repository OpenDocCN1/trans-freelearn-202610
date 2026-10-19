

+   - Office Scripts 与 Power Automate 深度集成——这是一个我们在下一章会介绍的工具，它将允许你自动化 Excel 中大量手动步骤，而不仅仅是 Excel。

使用 Office Scripts 有好处，但也有一些缺点：

- Microsoft 仍在推广 Office Scripts，从路线图来看尚不清楚它何时会面向个人（非商业）用户提供。
- Office Scripts 只能在 Excel 的网页版中编辑。实际上，编辑器足够好用（并且由更强大的 VS Code 在线编辑器支持），因此这不是一个主要问题。完成的脚本*可以*在网页版和桌面版 Excel 中运行。
- Office Scripts 没有 Script Lab 完整的用户界面选项。

最终，如何将 Excel 与你的决策服务链接的选择取决于你还想用该工具做什么。让我们通过参观 Office Scripts 来帮助你做出选择。

## 熟悉 Office Scripts 环境

要检查你是否可以使用 Office Scripts，请打开在线（Excel 365）版本。如果已由管理员启用，你应该会看到 **Automate toolbar** 选项，如 *Figure 7.13* 所示。

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_201_0.png)

Figure 7.13 – Office Scripts toolbar

Office Scripts 的大部分布局与 Script Lab 相似。工具栏具有 **New Script** 和 **Sample Script Areas**。**Automate a Task** 按钮链接到 Power Automate（更多内容请见 *Chapter 8*）。

**Record Actions** 按钮的工作方式与 VBA 中同名工具类似。点击它会捕获你在 Excel 中执行的操作，并以脚本形式显示在屏幕右侧的窗格中。以下脚本示例是我们设置 Excel 表格中某些值时录制的：

```
```typescript
function main(workbook: ExcelScript.Workbook) {
  let selectedSheet = workbook.getActiveWorksheet();
  // Set range B2 on selectedSheet
```
```

```
selectedSheet.getRange("B2").setValue("This is a test");
// Set fill color to FFFF00 for range B2 on selectedSheet
selectedSheet.getRange("B2").getFormat().getFill().
setColor("FFFF00");
}
```

屏幕右侧的窗格布局与 Script Lab 类似——包含代码编辑器、运行代码的日志消息区域以及多个有用的命令按钮。

代码编辑器具有保存按钮。文件保存在你的 OneDrive 文件夹下的 Documents | Office Scripts 中。这不是自动保存（不像 Script Lab），因此在运行前请确保已保存。

右侧面板中的 ... 菜单提供 Add 按钮选项。选择它（不出所料）会在当前工作簿中添加一个按钮来运行脚本，如 Figure 7.14 所示。

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_202_0.png)

Figure 7.14 – Sharing our script via a button in our workbook

更重要的是，创建此按钮会自动将我们的脚本与可以查看工作簿的人共享。这可能是确保工作簿和脚本一起分发的 easiest 方法。但在 ... 菜单的 Share 菜单选项中还有更详细的共享选项。

由于 Office Scripts 的大部分布局与 Script Lab 相似，我们保持描述简短。了解更多的一个良好起点是 Office Scripts 文档 https://learn.microsoft.com/en-us/office/dev/scripts/。

让我们通过修改我们之前的 Script Lab 示例使其在 Office Scripts 中工作来证明这两种脚本方法相似。

## 从 Office Scripts 调用我们的决策服务

当我撰写本节时，将之前的示例转换为在 Office Scripts 中工作不到 10 分钟。与其重复之前的步骤，我们将展示如何将更新的示例导入 Office Scripts，然后回顾并突出我们必须做出的更改：

1. 从通常的 GitHub 站点下载完整脚本。示例文件名是 07_Office_Script_Decision_Service.txt。请注意 .txt 扩展名，因为我们以两种不同格式提供了相同的示例。
2. 在 Notepad 等编辑器中打开文件并复制所有内容。
3. 在 Excel 的在线版本中，选择 Automate 菜单，然后选择 New Script，并粘贴文件内容以替换此窗口中的所有先前内容。
4. 保存结果。

如果你更熟悉 Office Scripts 保存文件的位置，也可以直接将示例下载到 OneDrive 文件夹下的 Documents | Office Scripts。请注意文件格式略有不同——你需要的是 07_Office_Script_Decision_Service.osts 文件。请注意 .osts 扩展名。

如果下载示例不起作用，请检查你是否使用了正确的方法对应的文件。

示例中最大的变化是由于 Office Scripts 没有用户界面元素，我们已将其从示例中删除。虽然我们将传递给决策服务的值硬编码以保持简单，但很容易改为从单元格值读取这些值。

脚本中其他值得突出的关键更改如下：

- Office Scripts 为我们隐藏了大量复杂性。我们有一个简单的 main 方法，它始终以 ExcelScript.Workbook 作为参数，以便脚本可以操作当前工作簿。
- 由于示例使用 TypeScript，我们需要标记脚本使用的对象的类型——因此，我们的 main 方法中包含 workbook 标签：
- 我们的脚本设置 URL、标头、JSON 有效负载和 URL 并使用 Fetch 执行实际的 REST 调用，与之前相同。我们也以相同的方式从返回的值中选择产品推荐。
- 用于（重新）创建工作表和标题并显示产品推荐的脚本大致相同。任何细微差异都是由于使用了略有不同的 Office.js API 版本。
- 错误处理已自动内置，因此我们无需编写自己的函数来执行此操作。

```
async function main(workbook: ExcelScript.Workbook) {
```

脚本注释良好，在按下 Run 按钮开始之前值得阅读一遍。点击按钮后，你应该会收到 Run Succeeded 消息，并且可以查看更新后的 Excel 表格，如 Figure 7.15 所示：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_204_0.png)

Figure 7.15 – Excel sheet online, updated with decision service results

由于我们保留了之前的示例中的日志记录，脚本输出信息窗口提供了大量关于刚刚发生的事情的信息，如 Figure 7.16 所示。同样，这些步骤与我们的 Script Lab 示例相同。

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_204_1.png)

Figure 7.16 – Script logging output

我们将在第 8 章再次回到 Office Scripts。但就目前而言，恭喜！如果你运行了此示例以及之前的示例，你现在就有四种不同的方式将 Excel 与你的 Kogito 决策服务链接（并且知道另外两种方法）。这现在给你一个困难的选择——你应该使用哪种方法？

## 选择很多——但该链接哪一个？

我们提到将 Excel 与决策服务链接的多种不同方法是有原因的。没有一种最佳方法——你的选择将取决于你希望在项目中实现的其他目标。此表可能有助于你决定：

| Tool | Advantage | Disadvantage | Use when |
|---|---|---|---|
| Power Query | Built into Excel. Acts as a powerful function. | Users can break it. No UI elements. | Default method to get started. |
| Visual Basic | Very powerful. Includes UI elements. | Security warnings scare users. Outdated. | Using a legacy code base or team has VBA. |
| Script Lab | Modern version of Visual Basic based on web technologies. | Needs installing. | Looking for a rich user experience or if Office Scripts is not available. |
| Functions in Script Lab | Easiest to use – feels like a normal Excel function. | Needs three items aligned to work (Script Lab, the function to be registered, and the function to be in the workbook). | Niche – consider Power Query instead. |
| Office Scripts | Simpler than Script Lab. No installation needed. | Not available to all users. No UI elements. | After you outgrow Power Query – if it is available to you. |
| Power Automate | External to Excel. Harder for users to break. | More powerful but more complicated. | Need to call the decision service as part of wider workflow. |

Table 7.1 – Different methods for linking Excel and your decision service

如果你发现难以选择，这里有个好消息——将业务规则从 Excel 移出并放入 Kogito 等决策服务的任何步骤都是一个好的步骤。

