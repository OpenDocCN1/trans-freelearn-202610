

```javascript
} catch (error) {
  // Note: In a production add-in, you'd want to
  // notify the user through your add-in's UI.
  console.error(error);
}
}
```

让我们逐步分析脚本中的关键点。实际上，你只需要编辑我们在*第 4 步*中突出显示的脚本来做你想做的事情，其他行都是标准模板：

1. 第一行，以 `$("#run")` 开始，使用 jQuery 将 HTML 页面上的按钮（称为 **Run**）与同名函数关联起来。
2. 你可能会认出 `function run()` 是定义当按钮被点击时将被调用的代码块（我们在上一行设置的链接）。async 关键字意味着不要等待函数完成——这样 Excel 在函数运行时不会冻结。
3. 下一行，以 `await Excel.run(` 开始，是 Excel 脚本中的标准写法。基本上，我们将接下来的几行脚本传递给 Excel 引擎，然后等待结果。
4. 我们的代码将 `Hello World` 输出到控制台。在此行前后有注释，以显示这是主要编辑区域，如果你以后想扩展示例。
5. 由于之前的行告诉我们并行运行多个项目（前一行中的 async），`await context.sync` 强制 Excel 暂停并让所有内容跟上。
6. 注意闭合括号与开头括号的顺序相反。你可能不需要编辑此部分，但如果你要编辑，请注意保持这些括号对齐。
7. 最后，有一个 `try/catch` 辅助函数。它捕获任何错误，以确保无论发生什么情况用户都能得到更新。你不太可能需要编辑它。

虽然我们从逐步讲解中知道应该发生什么，但让我们仍然运行脚本：

1. 从 Excel 顶部左侧的主 Script Lab 工具栏中，点击 **Run**。选择并排运行的选项。
2. 侧边栏应如 *图 7.8* 所示进行更新。注意两个 Script Lab 图标，它们使我们能够并排查看正在运行的项目和脚本编辑器，并在两者之间进行实时更新：

   ![](img/48815d6f06dc8c7aeaa3963d289fb1e7_195_0.png)

   图 7.8 – 运行 Hello World

3. 点击主窗格中的 **Run** 按钮。控制台应出现在侧窗格底部显示 **Hello World**，如 *图 7.9* 所示：

   ![](img/48815d6f06dc8c7aeaa3963d289fb1e7_196_0.png)

   图 7.9 – Hello World 结果

虽然这只是一个 Hello World 示例，但我们已经看到了标准 Script Lab 项目的结构。该结构不会改变——这使得我们下一步，从 Script Lab 调用决策服务，比写起来容易得多。

## 从 Script Lab 调用我们的决策服务

由于 Hello World 项目遵循标准的 Script Lab 格式，而我们对决策服务的调用也采用标准格式，因此不足为奇的是，当我们将两者结合起来从 Excel 中的 Script Lab 调用决策服务时，我们会熟悉大部分脚本。

与其重复大量熟悉的元素，我们建议你从 GitHub 书籍网站下载示例——它是 07_Script_Lab_Decision_Service.yaml。如前所述，将此文本在编辑器中打开，复制全部内容，然后打开 Script Lab 顶部左侧的汉堡菜单，选择 **Import**，并粘贴所有文本。

这样，我们就可以专注于以下逐步讲解中的有趣部分。

此文件中定义了用户界面的几个元素。在 Script Lab 中查看 **HTML** 选项卡，你可以看到客户号码的输入，如以下摘录所示。其他文本字段遵循类似的模式：

```html
<div class="ms-TextField">Customer Number
  <input type="number" id="cNumber" value="update me"/>
</div>
```

这些元素与其它 HTML 元素一起提供了如 *图 7.10* 所示的用户界面。为了保持简单，我们没有为所有可能传递给决策服务的信息包含字段——但如果你愿意，使用相同的模式添加它们是微不足道的。

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_197_0.png)

图 7.10 – 我们在 Script Lab 中决策服务示例的用户界面

让我们逐步介绍脚本选项卡中的行，并重点关注我们更改以使用 Script Lab 将 Excel 链接到决策服务的项目：

1. 与之前的示例一样，我们有一些注释和使用 console.log 的日志记录来解释正在发生的事情。
2. 我们将 url 设置为指向我们的决策服务。你需要更新此值。它与我们的 Power Query 示例相同；我们可以从 Swagger UI 获取。它是链接以 Customer Recommendations/Product Recommendation Service 结尾的链接。
3. 我们需要为 POST 调用设置 headers。我们将其定义为常量，因为调用 Kogito 决策服务时它们始终相同：

   ```javascript
   const headers = {
     Accept: "application/json",
     "Content-Type": "application/json"
   };
   ```

4. 我们定义了一个 payload，并带有默认值。这会生成与 Swagger UI 完全相同的 JSON。我们稍后将使用脚本更新这些默认值：

   ```json
   let payload = {
     Customer: {
       Number: 10000,
       Name: "Jane Doe",
       "Date of Birth": "2022 - 10 - 15",
       "Country of Residence": "NL",
       "Special Requests": "NA",
       "Previous Orders": [0]
     }
   };
   ```

5. 我们使用默认有效负载中的值更新用户界面。`$("#cNumber")` 是 jQuery 查找 HTML 字段的方式。`.val` 使用我们之前定义的负载 JSON 设置值。我们对所有字段重复此模式：

   ```javascript
   $("#cNumber").val(payload.Customer.Number);
   ```

6. 如 `Hello World` 示例中所示，我们将按钮链接到在按钮被按下时运行我们的主函数 `callDecisionService`：

   ```javascript
   $("#get-product-recommendation").click(() =>
   tryCatch(callDecisionService));
   ```

7. `callDecisionService` 使用与传递给 Excel 的相同 async 代码定义，正如我们在前一个示例中看到的那样：

   ```javascript
   async function callDecisionService() {
     await Excel.run(async (context) => {
   ```

8. 以防用户表单上的值已更改，我们用它们更新我们的 JSON 有效负载。这是步骤 5 的逆过程；我们对所有字段重复。`+` 符号强制值转换为数字：

   ```javascript
   payload.Customer.Number = +$("#cNumber").val();
   ```

9. 对决策服务的 `fetch` 调用被日志记录包围，这样我们可以看到传递到服务中和从服务返回的值。作为我们调用的一部分，我们使用之前设置好的 URL、headers 和 JSON 有效负载：

   ```javascript
   const response = await fetch(url, {
     method: "POST",
     headers: headers,
     body: JSON.stringify(payload)
   });
   const jsonResponse = await response.json();
   ```

10. 我们的决策服务返回大量我们不需要的 JSON 信息。我们使用 `let productRecommendation = jsonResponse["decisionResults"][0]["result"];` 获取我们想要的具体决策信息。

值得下载并再次浏览脚本。只需做一些小的更改，它就可以用来调用你自己的决策服务。我还鼓励你查看内置的 Script Lab 示例。它们涵盖了你可能想要用更复杂的用户界面元素扩展项目的大多数方式。

## 替代方法 – Script Lab 中的自定义函数

当你浏览示例时，你可能会注意到使用 Script Lab 创建自定义函数的选项。乍一看，这似乎很有吸引力——它允许你构建自己的专用函数，然后直接在 Excel 单元格中输入如下内容：

```
= callDecisionService (customerNumber, countryOfResidence, dateOfBirth ..)
```

然而，要使这种方法有效，你需要安装 Script Lab、注册自定义函数，并将函数输入到电子表格中。如果你错过了这三个步骤中的任何一个，你的电子表格就会损坏。

对于大多数用例，我们在上一章中介绍的 Power Query 方法更稳健。它在许多方面都像自定义函数一样运行，它内置于 Excel 中，并且不需要在首次使用时运行以将其与 Excel 注册。

关于 Script Lab 还有很多东西可以学习，它可以为你提供非常强大的基于 Excel 的解决方案。由于它和我们将在下一节中介绍的 Office Scripts 有很多共同点，因此花时间浏览 Microsoft 文档是一个很好的投资，网址为 https://learn.microsoft.com/en-us/office/dev/add-ins/reference/overview/excel-add-ins-reference-overview。如果你想深入研究任一工具，我还推荐以下书籍，以充分探索使用这些工具扩展 Excel 的强大功能 – https://leanpub.com/buildingofficeaddins。

## 什么是 Office Scripts？

虽然所有 Excel 用户都可以访问 Script Lab，但并非每个人都能访问 Office Scripts——目前还不能。由于我们已经知道它共享 Office.js 通过脚本操作 Excel 的方法，两者之间有什么区别？

- 虽然与 Script Lab 非常相似，但 Office Scripts 更易于使用。例如，一些样板 async 调用和错误处理函数对我们来说是隐藏的。
- Office Scripts 具有记录器，可以记录我们在 Excel 中的操作并将其转录为脚本。这对于学习 Office.js 特别有用。
- Office Scripts 预装在 Office 365 在线版中——尽管可能需要管理员启用。
- 更容易共享脚本。虽然（像 Script Lab 一样）脚本位于工作簿之外，但通过向工作表添加按钮，任何可以看到 Excel 表的人也可以看到脚本。

