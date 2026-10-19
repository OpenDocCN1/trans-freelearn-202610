

- IBM 和 Red Hat 在安全、客户保密以及确保符合 GDPR 方面花费了大量时间。这些厂商将声誉押在保持数据安全上，而这种安全级别并非每个小型公司（包括你的公司）都能达到。
- 本书提倡更正式的数据管理技术和决策方法。我们正从组织内散落的 Excel 电子表格（数据管理噩梦）转变为以连贯的方式存储和处理它们。这种方法也使 GDPR 合规变得更容易。

从良好的数据管理习惯开始，将使你的生活在以后变得轻松得多。另一个你以后会庆幸的好习惯是备份并对决策模型进行版本控制。让我们看看 KIE Sandbox 与 GitHub 的内置集成是如何让我们做到这一点的。

## 使用 GitHub 保存和共享你的决策模型

你可能没有太在意（除了 KIE Sandbox 中的 **Ephemeral** 消息），但你目前是将决策模型保存在与你的网络浏览器关联的本地存储中。虽然这对于入门来说很棒，但我们不建议将其用于长期使用。如果你的笔记本电脑丢失了——你有备份吗？你如何与同事共享决策模型？如果意识到自己犯了错误，想恢复 5 分钟前的版本怎么办？本地存储很难让你完成这些事情。

幸运的是，KIE Sandbox 提供了通往最佳协作网站的链接：GitHub。你可能还记得在 *第 2 章* 中我们介绍 KIE 和 Drools 项目时提到的 GitHub，它让数百名开发者能够协作、共享文件并浏览他们工作的不同版本。

GitHub 提供了私有仓库选项，你可以将其用于自己的项目并限制为仅希望协作的人员。需要注意的是，你的仓库是你的空间——它与托管在同一站点上的任何 Red Hat 项目无关，但这意味着我们需要设置自己的空间并将其链接到 KIE Sandbox。现在让我们逐步进行。

## 注册 GitHub 并获取你的 token

要使用 GitHub，你需要注册一个账户。该网站由 Microsoft 所有，并且其其他网站的登录（目前）还不可用。要获取你的 GitHub 登录，请执行以下操作：

1. 前往 GitHub 主页 `www.github.com`。
2. 你会看到一个欢迎页面，其中有一个大的 **Sign up for GitHub** 按钮。
3. 点击此按钮将带你进入 *图 5.16* 中的账户注册屏幕：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_139_0.png)

图 5.16 – GitHub 注册屏幕

4. 在输入你的详细信息（包括新用户名和电子邮件地址）后，点击 **Create Account** 按钮。
5. 你会收到一封验证电子邮件地址的消息。点击该邮件中的链接将带你进入 GitHub 上的个人主页。

## GitHub 上个人主页的导览

你在 *图 5.17* 中看到的页面与你在 *第 2 章* 中看到的 Kogito/Drools 项目页面不同。这是你在 GitHub 上的个人页面，提供了探索你可能想要交互的多个项目的选项。

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_140_0.png)

图 5.17 – 你的个人 GitHub 主页

这里有一个创建仓库的选项（使用此页面底部的绿色按钮），用于存储我们的决策模型文件，但 KIE Sandbox 也能为我们做到这一点（我们将在本章后面看到具体如何操作）。让我们专注于将 KIE Sandbox 与我们刚刚创建的 GitHub 仓库链接所需的步骤。

要创建 KIE Sandbox 与 GitHub 之间的链接，我们需要在 GitHub 上拥有一个个人 token。就像 OpenShift 一样，这个 token 替代了我们的用户名和密码，以便 GitHub 知道 KIE Sandbox 获准代表我们行事。要创建此 token，请按照以下步骤操作：

1. 在 GitHub 的右上角（任何屏幕），点击个人资料设置，如 *图 5.18* 所示。

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_140_1.png)

图 5.18 – 你的 GitHub 个人资料设置

2. 搜索 **Developer settings**（左下角，列表末尾），然后像 *图 5.19* 中那样突出显示 **Personal access tokens**。

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_141_0.png)

图 5.19 – GitHub 的 Personal access tokens 屏幕

3. 点击 **Generate new token** 按钮，然后在提示时确认你的 GitHub 密码。

4. *图 5.20* 中的屏幕将会出现，允许你设置与此 token 关联的权限。权限必须包括仓库范围，以便当 KIE Sandbox 使用 token 时，它可以创建新的仓库来存储模型。请注意，还有更多选项，但我们已裁剪图像以适应页面。

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_141_1.png)

图 5.20 – 为新的个人访问 token 设置权限

立即复制屏幕上出现的 token（如下图 5.21 所示），因为它不会再次显示。我们很快就会在下一节的第一步中使用它，将 token 添加到 KIE Sandbox 以将其链接到我们的 GitHub 账户。

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_142_0.png)

图 5.21 – 复制新生成的个人访问 token

> **关于 GitHub token 的更多信息**
> 我们只会给你足够的信息来使用 KIE Sandbox 创建与 GitHub 的链接，但如果你想了解更多关于 GitHub token 的信息，一个不错的起点是 https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token。

## 使用你的 token 在 KIE Sandbox 上协作

由于我们已经设置了 GitHub 账户并记下了唯一的访问 token，我们希望将 KIE Sandbox 与其链接：

1. 你需要先在常用页面 https://sandbox.kie.org 上打开 KIE Sandbox。
2. 点击右上角的人物图标选择 Connected Accounts，然后选择连接 GitHub 账户的选项。
3. 在此屏幕上粘贴你的 GitHub token。粘贴 token 后，你应该会收到一条确认连接的消息（如图 5.22）：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_143_0.png)

图 5.22 – 成功链接 KIE Sandbox 和 GitHub

4. 在收到此成功消息后，点击右上角的 x 关闭弹出窗口。

5. 现在，如果我们点击主 KIE Sandbox 右上角的 **Share** 菜单，你会看到我们有额外的选项，如 *图 5.23* 所示。请注意，我们为节省空间移除了几项菜单项，因此你可能需要在屏幕上向下滚动才能看到我们突出显示的选项，当然，你的 GitHub 用户名会有所不同。

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_143_1.png)

图 5.23 – 使用我们新创建的 GitHub 连接进行共享

6. 向下滚动并选择你刚刚连接的 GitHub 账户。

现在 KIE Sandbox 和 GitHub 已链接，让我们利用该链接解决之前看到的一些问题。

## 用于保存和项目协作的 GitHub 流程

一旦 KIE Sandbox 链接到 GitHub，我们将在屏幕右上角的 Share 菜单中拥有更多选项。由于我们担心本地存储，我们希望将决策模型保存在更稳健的地方：

1. 在 Share 菜单中，我们现在有了 Create repository 选项。点击它，你将看到如图 5.24 所示的选项屏幕：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_144_0.png)

图 5.24 – KIE Sandbox 在 GitHub 中为我们的决策模型创建仓库

2. 输入仓库名称并选择创建为私有的选项（我们以后可以更改此设置）。

3. 经过几秒钟，你会看到一条确认消息，表明仓库已创建以及可以查看文件的链接，如图 5.25 所示：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_144_1.png)

图 5.25 – 仓库已成功在 GitHub 中创建

