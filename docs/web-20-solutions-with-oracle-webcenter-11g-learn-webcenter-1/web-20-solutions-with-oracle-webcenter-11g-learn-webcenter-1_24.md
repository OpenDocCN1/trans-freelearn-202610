# 使用 Oracle Composer 进行个性化

传统上，只有技术专业人员负责设计和维护公司网站的信息。这是因为构建网页需要高水平的技术专业知识。

企业门户的出现促进了用户在信息设计和内容方面的参与。然而，当时可用的技术（HTML 和 JavaScript）无法在内容和表现方面开发丰富的用户界面。

新技术（AJAX、XML、Web 2.0、RSS 等）的到来有助于改进门户的维护和创建。

Oracle Composer 是 WebCenter 的关键组件之一，它利用这些新技术，使用户能够以友好且直观的方式改进其界面的设计和内容。在本章中，我们将分两部分使用 Oracle WebCenter：

- 在第一部分中，作为开发人员，我们将为最终用户准备环境。
- 然后，我们将扮演最终用户的角色，对前一步中定义的页面进行个性化设置。

### 面向开发人员的 Oracle Composer

Oracle Composer 为最终用户提供了高度功能化的设计环境。为了实现这一点，开发人员需要准备一个环境，最终用户将使用该环境来设计其内容。

接下来我们将展示的只是开发人员用于准备此类环境的 Oracle Composer 组件。

我们将用于准备此环境的工具是 Oracle JDeveloper，它帮助我们定义区域，最终用户将随后使用这些区域来个性化其内容。

## Oracle 组件类型

Oracle 组件可按功能分为以下类型：

- 常见组件
  - 更改模式按钮：这是一个按钮，允许用户从运行时模式切换到设计模式。
  - 更改模式链接：此链接与前一个组件具有相同的用途。
  - 图像链接：此组件提供带有关联图像的链接。
  - 页面可自定义：使用此组件，我们可以创建一个区域，其内容可以由用户动态更改。
- 布局组件
  - 面板可自定义：此面板充当用户在运行时模式下选择的组件的容器。
  - 布局可自定义：这是另一种容器，其特点是允许用户以多种方式对其进行划分。
  - 显示详细信息框架：此容器的特点是提供类似于端口的区域。它可以用于包含一个项目。
- 操作组件。
  - 自定义操作：此组件可以调用任务流。

## 使用 Oracle Composer 构建环境

在最终用户可以自定义其内容之前，开发人员需要准备动态环境。为了实现这一目标，开发人员需要执行以下步骤：





## 1. 执行 JDeveloper，并在 Application Navigator 中选择 Paracas。

![](img/fccf6c511126cf79e98510edd3b343ac_225_0.png)

## 2. 右键单击 ViewController 项目并选择 New。接下来，在 New Gallery 窗口中，选择类别 Web Tier | JSF，选择选项 JSF Page，然后单击 OK 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_225_1.png)

3. 对于下一步，检查 **Create as XML Document** 选项，为页面定义名称 customInformation.jspx，选择 **Page template** 选项，选择 templateParacas，然后单击 OK 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_226_0.png)

对于使用 Oracle Composer 的页面，组件必须使用 .jspx 扩展名创建。

4. 页面创建完成后，我们将只使用区域 customRight。因此，在组件调色板中选择类别 ADF Faces，并将 **Panel Box** 控件拖动到区域 customRight 上，以定义一个容器。

![](img/fccf6c511126cf79e98510edd3b343ac_226_1.png)

## 5. 转到 Property Inspector 并更改 Custom Information 的 Text 属性。

![](img/fccf6c511126cf79e98510edd3b343ac_227_0.png)

## 6. 接下来，我们将在 panel box 中包含一个链接，允许用户在页面上激活设计模式。因此，在 Component Palette 中，选择 Oracle Composer 类别并拖动 Change Mode Link。

![](img/fccf6c511126cf79e98510edd3b343ac_227_1.png)

## 使用 Oracle Composer 进行个性化设置

7. 现在是时候定义用户可以自定义的区域了。这可以通过将 **Page Customizable** 组件拖动到上一张截图中定义的区域来实现。

![](img/fccf6c511126cf79e98510edd3b343ac_228_0.png)

8. 转到调色板结构，注意已生成 Page Customizable 组件。此外，在此组件下生成了一个 **panelCustomizable** 容器。

![](img/fccf6c511126cf79e98510edd3b343ac_228_1.png)

9. Panel Customizable 容器不允许用户以其他方式划分区域。出于这个原因，我们正在删除该容器，并将其替换为具有该功能的 Layout Customizable 容器。因此，首先通过右键单击容器并选择 **Delete** 选项来删除容器 **Customizable Panel**。

10. 现在，将 **Layout Customizable** 组件拖动到控件 Page Customizable 内，如下一张截图所示：

![](img/fccf6c511126cf79e98510edd3b343ac_229_1.png)

## 11. 通过右键单击页面并选择 **Run** 按钮来测试页面。

![](img/fccf6c511126cf79e98510edd3b343ac_230_0.png)

最后，它将显示下一页。如您所见，我们现在在运行时模式下有一个基本页面。当用户单击编辑链接时，页面将更改为编辑模式，因此用户可以自定义其内容。

![](img/fccf6c511126cf79e98510edd3b343ac_230_1.png)

## 面向最终用户的 Oracle Composer

一旦环境准备就绪，就是最终用户自定义其内容的时候了。

在执行自定义之前，了解 Oracle Composer 背后的以下技术很重要。

Oracle Metadata Service 和 Oracle Composer 允许每个用户自定义页面的内容。这样，每个用户都将拥有基础页面的不同版本。
您可能会问：这如何作用于 Oracle Composer？答案是归功于 Oracle Metadata Service。
这项技术允许我们将任何用户自定义存储在基于 XML 的存储库中。也就是说，在运行时，用户所做的任何自定义都不会更改应用程序代码，而是存储在与应用程序每个用户关联的 XML 文件中。

## 资源目录

资源目录包含最终用户可以在设计模式下用于自定义其内容的组件。
通常，工具提供一组默认组件，但可以对其进行配置以公开 portlets、task flow、JSF 组件和其他类型的组件。

![](img/fccf6c511126cf79e98510edd3b343ac_231_0.png)

## 使用 Oracle Composer 自定义内容

使用先前开发的示例，我们将自定义页面内容。
请注意，页面必须处于运行时模式。

1. 首先，让我们将页面更改为设计模式。为此，请单击 **Edit** 链接。

![](img/fccf6c511126cf79e98510edd3b343ac_232_0.png)

2. 现在显示的是页面的设计环境。在此模式下，用户可以自定义我们在本章第一部分中准备的区域的内容。

![](img/fccf6c511126cf79e98510edd3b343ac_232_1.png)

3. 在我们的情况中，我们将自定义页面内容以报告在服装制作中使用天然纤维的影响。因此，最初我们将更改工作区的标题。为此，请单击 **Page Properties** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_233_0.png)

4. 您将看到一个屏幕，您可以在其中更改工作区的标题。将标题更改为 Natural Fibers - Cotton, Wool 并按 OK 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_233_1.png)

5. 现在让我们通过单击 Change Layout 按钮并选择 Two columns, narrow right 来划分工作区，如下一张截图所示：

![](img/fccf6c511126cf79e98510edd3b343ac_233_2.png)

## 使用 Oracle Composer 进行个性化设置

6. 经过上述更改，已创建两个区域。现在，我们将通过单击该区域的 **Add Content** 按钮来填充左区域的内容。

![](img/fccf6c511126cf79e98510edd3b343ac_234_0.png)

7. 以下窗口将显示组件目录。单击类别 **ADF Faces Components** 以显示可用组件。

![](img/fccf6c511126cf79e98510edd3b343ac_234_1.png)

8. 从可用组件列表中，HTML Markup 组件允许我们直接在页面上包含 HTML 和 JavaScript 内容。为此，请单击 **Add** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_234_2.png)

9. 最后单击 **Close** 按钮。

10. 请注意，页面中已包含一个 HTML Markup 控件。现在我们将更改其属性；为此，请单击 **Edit** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_234_3.png)

11. 现在，我们将在此处包含一个与我们页面内容相关的 YouTube 视频引用。为此，请打开另一个浏览器窗口并找到以下视频：http://www.youtube.com/watch?v=KuwBcycCM0w

12. 单击 **Embed** 按钮以查看允许我们在网站上包含此视频的代码。

![](img/fccf6c511126cf79e98510edd3b343ac_235_0.png)

13. 复制视频的代码。

![](img/fccf6c511126cf79e98510edd3b343ac_235_1.png)

14. 返回设计的页面，并将复制的代码粘贴到值字段中。更改值属性后，单击 **OK** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_235_2.png)

15. 设计的页面将如下所示：

![](img/fccf6c511126cf79e98510edd3b343ac_236_0.png)

16. 我们现在要填充右区域。为此，请单击该区域的 **Add Content** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_236_1.png)

17. 选择 Image 组件，它允许我们在页面上显示一张图片。为此，请单击 **Add** 按钮并关闭 Catalog 窗口。

![](img/fccf6c511126cf79e98510edd3b343ac_236_2.png)

18. 经过上述更改，我们的网站上添加了一张图片。现在，让我们设置该图片。为此，请单击 **Edit** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_237_0.png)

19. 将 **Image Source** 属性更改为与显示天然纤维产量增长的图片相关的以下 URL。
`http://www.fao.org/fi/figis/graphs/species/Aquaculture/2965.gif`

![](img/fccf6c511126cf79e98510edd3b343ac_237_1.png)

20. 按 **OK** 按钮完成更改。
21. 为了撰写关于视频和图片含义的信息，请在右区域单击 **Add Content** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_237_2.png)





# 使用 Oracle Composer 进行个性化设置

22. 在目录窗口中，单击 **Add** 按钮选择 **Text** 控件。

![](img/fccf6c511126cf79e98510edd3b343ac_238_0.png)

23. 单击 **Close** 按钮关闭 **Component** 目录。

24. 请注意，页面上已添加一个文本组件；单击链接 **Edit Text** 对其进行自定义。

![](img/fccf6c511126cf79e98510edd3b343ac_238_1.png)

25. 接下来，将出现一个编辑区域。

![](img/fccf6c511126cf79e98510edd3b343ac_238_2.png)

26. 编写自定义注释，然后单击链接 **Done Editing**。

![](img/fccf6c511126cf79e98510edd3b343ac_239_0.png)

27. 最后，要在执行模式下查看所有自定义内容，请单击页面右上角的 **Close** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_239_1.png)

28. 我们的页面应如下所示：

![](img/fccf6c511126cf79e98510edd3b343ac_239_2.png)

## 摘要

Oracle Composer 为内容个性化开辟了许多可能性。这项技术帮助最终用户创建自己的内容，并将其与外部信息相结合。这样一来，他们可以将知识共享和转移给公司所有成员，从而提高组织的生产力。

在接下来的章节中，我们将在名为 Oracle Spaces 的产品中使用这项技术，并学习最终用户如何继续生成自己的内容。

29. 从现在起，每次重新运行页面时，您都会发现更改已保留。您可能会想，这些更改在哪里？答案很简单，在项目的元数据中。也就是说，在应用程序中生成的一个 XML 文件中。您可以在以下路径中找到此文件：
C:\Paracas\mds\mdssys\cust\site\site\customInformation.jspx.xml

30. 您可以使用记事本打开该文件，找到用户所做的所有更改。

31. 让我们使用 Windows 资源管理器删除元数据文件，以删除用户自定义内容。

32. 再次执行页面，您应该看到没有任何自定义内容的相同页面。

![](img/fccf6c511126cf79e98510edd3b343ac_240_0.png)
