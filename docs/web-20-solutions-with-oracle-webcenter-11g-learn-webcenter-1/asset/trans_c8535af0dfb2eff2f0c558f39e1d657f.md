

5. 在登录页面，输入用户名：eduardo.pulido 和密码：oracle。单击 Login。

![](img/fccf6c511126cf79e98510edd3b343ac_247_0.png)

6. 接下来，您可以看到 Oracle Personal Spaces 环境。

![](img/fccf6c511126cf79e98510edd3b343ac_247_1.png)

7. 在左侧，Oracle Personal Spaces 显示名为 Sidebar 的工具栏。此工具可快速访问 Oracle WebCenter Spaces 提供的资源。

![](img/fccf6c511126cf79e98510edd3b343ac_248_0.png)

8. Sidebar 的元素如下：

- **Application**：提供对用户使用的业务应用程序的访问。它包括用于保存密码的 Single Sign-On。
- **Worklist**：用户通常参与工作流，例如审批和评审。此任务流或端口将提供对待用户审批的流程的访问。
- **Mail**：链接到用户电子邮件或第三方电子邮件（如 Yahoo 和 Google）以及其企业电子邮件。
- **Buddies**：访问其即时消息联系人列表。
- **Notes**：允许用户创建简短笔记。
- **Recent Documents**：用户最近访问的文档将在此区域显示。
- **Discussions**：访问相关的热门和收藏话题。
- **Tags**：通过标签提供对页面的访问，并提供直接访问 Tag Center 的途径。

9. 在右侧，Oracle Personal Spaces 显示连接用户所属的页面。请注意，默认情况下有两个页面，**Welcome** 和 **Documents**，它们通过选项卡显示。

10. 第一个页面是信息页面，第二个页面允许我们访问在上一步中配置的文档存储库。

![](img/fccf6c511126cf79e98510edd3b343ac_248_1.png)

## 11. 要配置其页面，用户可以使用选项 Page Actions | Manage Pages…。

![](img/fccf6c511126cf79e98510edd3b343ac_249_0.png)

请注意，此窗口允许您管理所有用户页面。此外，用户还可以访问 WebCenter Spaces 中的 Role Based Pages。这些是分配给公司中角色的页面。因此，如果用户属于销售角色，则他或她将自动拥有仅限于该角色的页面的访问权限。

## 12. 让我们使用此功能创建一个新页面。单击 Create Page 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_249_1.png)

## 13. 输入页面名称为 Custom Information，方案为 Dunes。单击 Create 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_249_2.png)

# Oracle WebCenter Spaces Overview

14. 通过单击此链接更改页面布局：**Change Layout | Two columns above…**。

![](img/fccf6c511126cf79e98510edd3b343ac_250_0.png)

15. 请注意，我们现在有三个区域可供用户使用，以放置与购买天然纤维相关的数据。首先，在左上角，单击按钮 **Add Content**。

![](img/fccf6c511126cf79e98510edd3b343ac_250_1.png)

16. 在窗口组件目录中，单击 **Layout** 选项。

![](img/fccf6c511126cf79e98510edd3b343ac_250_2.png)

17. 在下一个窗口中，单击与 Text 控件关联的 **Add** 按钮，然后按 **Close** 按钮关闭组件目录。

![](img/fccf6c511126cf79e98510edd3b343ac_251_0.png)

18. 请注意，您已在此新页面上创建了一个文本组件。接下来，单击 **Edit Text** 链接并添加有关市场上天然纤维类型的信息。您可以选择并格式化任何文本段落。通过单击 **Done Editing** 链接完成此更改。

![](img/fccf6c511126cf79e98510edd3b343ac_251_1.png)

19. 现在，在右上部分，我们将放置有关世界天然纤维可用性的新闻。为此，我们将使用 RSS 服务。接下来，单击右上区域中的 **Add Content** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_251_2.png)

20. 单击 RSS 控件的 **Add** 链接。然后，按 **Close** 按钮关闭目录。

![](img/fccf6c511126cf79e98510edd3b343ac_251_3.png)

# Oracle WebCenter Spaces Overview

21. 创建 RSS 控件后，您必须对其进行配置。为此，单击 **Edit** 链接（铅笔图标）。

![](img/fccf6c511126cf79e98510edd3b343ac_252_0.png)

22. 完成 **RSS Feed URL** 字段，填入包含所需信息的地址。在我们的情况中，我们使用以下 URL：
`http://rss.globalsources.com/I/Cotton-fiber-manufacturers/r/2000000003844/3000000179301/21806/RSS.xml.`

![](img/fccf6c511126cf79e98510edd3b343ac_252_1.png)

23. 按 **OK** 应用更改。
24. 最后，在页面的下部区域，用户将放置用户在采购过程中使用的文件。为此，单击底部的 **Add Content** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_252_2.png)

25. 选择类别 **Documents**。

![](img/fccf6c511126cf79e98510edd3b343ac_252_3.png)

26. 单击 **Documents** 的 **Add** 链接并按 **Close** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_252_4.png)

# Chapter 10

27. 现在自定义此控件以仅显示用户的目录。为此，单击与 Document 控件关联的 **Edit** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_253_0.png)

28. 在 **Start Folder Path** 字段中，单击 **Edit** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_253_1.png)

29. 在下一个屏幕中选择值，以定义带有用户名的目录作为存储库的根。在打开的窗口中按 **OK**。

![](img/fccf6c511126cf79e98510edd3b343ac_253_2.png)

30. 最后，单击页面右上角的 **Close** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_253_3.png)

31. 通过单击 **Save** 按钮接受对页面所做的更改。

![](img/fccf6c511126cf79e98510edd3b343ac_253_4.png)

## 32. 最后，您将在用户页面上欣赏以下更改。

![](img/fccf6c511126cf79e98510edd3b343ac_254_0.png)

![](img/fccf6c511126cf79e98510edd3b343ac_254_1.png)

# Building a business role page

如我们在介绍中提到的，业务角色页面是一种可以包含在所有具有特定角色的用户工作环境中的页面。这样，我们可以将信息定向到预定义的用户组，避免逐一分配的需要。

在此示例中，我们将定义角色 Purchaser 并将用户分配给该角色。然后，我们将创建一个列出公司最佳供应商的业务角色页面，该页面将对 Purchaser 角色的所有用户可用。

# Hands on

- 1. 转到 Oracle Spaces 登录页面：`http://localhost:8888/webcenter`。
- 2. 与创建 `eduardo.pulido` 用户的方式相同，单击 Register 按钮并继续创建用户 `martin.alban` 和 `diego.munoz`，两者的密码均为 `oracle`。

![](img/fccf6c511126cf79e98510edd3b343ac_255_0.png)

- 3. 注册后，返回初始页面并以管理员用户身份登录，在我们的情况中，用户为 **weblogic**。

![](img/fccf6c511126cf79e98510edd3b343ac_255_1.png)

# Oracle WebCenter Spaces Overview

4. 进入主环境后，单击 **Administration** 链接。

![](img/fccf6c511126cf79e98510edd3b343ac_256_0.png)

5. 在显示的窗口中，选择选项 **Manage WebCenter Users and Roles**。

![](img/fccf6c511126cf79e98510edd3b343ac_256_1.png)

6. 然后，将显示用于定义 Oracle Spaces 使用的角色和用户的安全选项卡。单击 **Create Role** 链接。

![](img/fccf6c511126cf79e98510edd3b343ac_256_2.png)

# Chapter 10

7. 接下来，输入 **Purchaser** 并选择模板 **Spaces-User**。按 **OK**。

![](img/fccf6c511126cf79e98510edd3b343ac_257_0.png)

8. 创建角色后，我们将注册属于该角色的用户。为此，单击 **Users** 选项卡。

![](img/fccf6c511126cf79e98510edd3b343ac_257_1.png)

9. 此页面定义属于角色的用户。输入 **eduardo.pulido**，选择角色 **Purchaser**，并单击 **Grant Access**。这样，用户即与角色关联。

![](img/fccf6c511126cf79e98510edd3b343ac_257_2.png)

10. 对用户 **martin.alban** 和 **diego.munoz** 重复此操作。

11. 注册用户后，页面底部应显示以下信息。

![](img/fccf6c511126cf79e98510edd3b343ac_257_3.png)

