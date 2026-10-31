
此外，从 [https://github.com/PacktPublishing/GitHub-for-Next-Generation-Coders/tree/main/Chapter%209](https://github.com/PacktPublishing/GitHub-for-Next-Generation-Coders/tree/main/Chapter%209) 下载名为 `Chapter 9` 的 GitHub 文件夹，其中在 `.github/workflow` 文件夹内可以找到 `static.yml` 文件，您可以用来跟随操作。

# 了解 GitHub Actions 工作流

你和朋友们已经搭建了一个很棒的单页作品集网站。现在，你想添加一个名为 **MY ARTICLES** 的新板块来展示你的博客文章。你可能听说过 GitHub Actions 可以帮助你自动化一些任务，但这是什么意思？让我们简单地拆解一下。

![图 9.1 – MY ARTICLES 部分](image/B21847_09_1.jpg)
图 9.1 – MY ARTICLES 部分

*什么是 GitHub Actions？* 把 **GitHub Actions** 想成一个可以帮助你完成任务的有用机器人。例如，每当你想在 **MY ARTICLES** 板块中添加新文章时，这个机器人可以自动用新文章更新你的网站。就像有一个小助手帮你处理重复性任务，而无需你手动操作。但如果你不确定如何为我们刚才提到的那个有用的机器人编写指令怎么办？好消息是你不必从零开始。GitHub 有一个名为 **Marketplace** 的地方，你可以在那里找到其他聪明人已经制作好的操作。就像去商店挑选一个机器人，而不是自己建造一个。

GitHub Actions 工作流文件是用 **YAML** 编写的，其全称是 **YAML Ain’t Markup Language**。它是一种人类可读的数据序列化标准，这意味着它设计得易于人类阅读和编写。**workflow** 是你在 GitHub 仓库中定义并存储在仓库的 `.github/workflows` 目录中的自动化流程。工作流可以由不同的事件触发，例如向仓库推送代码、创建拉取请求或按计划执行。

让我们快速查看一下在 *图 9.2* 中部署网站到 GitHub Pages 的工作流语法。

![图 9.2 – GitHub Actions Marketplace](image/B21847_09_2.jpg)
图 9.2 – GitHub Actions Marketplace

让我们更详细地了解前述图中的每个术语：

*   **事件**：GitHub Actions 中的事件是触发自动化工作流的特定活动。例如，当有人向仓库推送代码或创建拉取请求时，它可以自动启动诸如测试代码之类的操作。这就像引发多米诺骨牌效应，一个操作导致另一个操作。以下是你可以在 GitHub Actions 中使用的一些事件：
    *   `on`：这部分告诉我们何时运行此工作流。就像设定特定时间开始烹饪一样。
    *   `push`：这告诉 GitHub 操作，“*当有人向 main 分支推送代码时启动工作流*”。在烹饪的语境中，这就像说，“*当主菜准备好时，就该上菜了。*”
    *   `workflow_dispatch`：这允许你手动启动工作流。就像在厨房里有一个按钮，可以在你准备好时随时启动烤箱。
*   **作业**：这概述了需要完成的工作。
*   **运行器**：这告诉我们操作将在哪个系统上执行。
*   **步骤**：GitHub Actions 中的步骤是构成工作流中作业的单个任务。这就像食谱中的步骤，每个步骤做一件具体的事情，例如混合配料或烘烤它们，以实现制作蛋糕的整体目标。以下是你可以使用的一步：
    *   **检出**：
        *   **它做什么**：此步骤从你的仓库获取最新代码。
        *   **工作原理**：`uses: actions/checkout@v4` 行告诉 GitHub 使用 `checkout` 操作，这是一个来自 GitHub Marketplace 的预构建操作。此操作检出你的仓库，以便工作流可以访问代码。
    *   **设置 Pages**：
        *   **它做什么**：使用必要的设置准备 GitHub Pages。
        *   **工作原理**：`uses: actions/configure-pages@v4` 行表示使用来自 Marketplace 的 `configure-pages` 操作。此操作配置你的仓库以准备 GitHub Pages 部署。
    *   **上传制品**：
        *   **它做什么**：收集仓库中的所有文件以进行部署。
        *   **工作原理**：`uses: actions/upload-pages-artifact@v3` 行指定使用来自 marketplace 的 `upload-pages-artifact` 操作。此操作上传需要部署的文件。`with` 关键字允许你向此操作传递参数。在这里，`path: '.'` 意味着包含当前目录中的所有文件。
    *   **部署到 GitHub Pages**：
        *   **它做什么**：将你的网站部署到 GitHub Pages。
        *   **工作原理**：`uses: actions/deploy-pages@v4` 指定行使用来自 Marketplace 的 `deploy-pages` 操作，该操作处理部署。`id: deployment` 为此步骤提供一个标识符，如果需要以后引用，可以使用它。
*   **操作**：操作就像自动化助手，可以在你的 GitHub 项目中为你完成任务，我们将在下一节中更详细地学习。

此工作流是一组指令，告诉 GitHub 如何在你对项目进行更改时自动更新你的网站，具体来说是在你向 main 分支推送新更改或手动触发部署时。所有这些都是为了使更新网站的过程像按一个按钮一样简单。

现在你已经尝到了 GitHub Actions 可以如何简化更新你的单页作品集网站的滋味，是时候深入挖掘了。接下来，我们将探讨如何创建自己的操作工作流。在这里，你将学习如何为你的有用的机器人编写脚本，为其提供详细指令，以便在你添加新功能时自动构建你的网站。

# 创建操作工作流

在你的 GitHub 仓库页面上，有一个标签名为 **Actions**。点击它后，你会在 GitHub Starter 页面上看到搜索工作流的选项。你也可以在 GitHub Marketplace 上搜索 GitHub Actions 并配置你需要的操作。想象一下走进一家机器人商店，看到所有可以使用的不同机器人的目录。操作就像那些不同的小机器人，你可以用它们自动运行项目的不同部分，例如检查代码中的错误或在你对项目进行更改时将网站部署到网上。

![图 9.3 – GitHub Actions 启动页面](image/B21847_09_3.jpg)
图 9.3 – GitHub Actions 启动页面

## 探索预构建操作

在 *图 9.4* 所示的 Marketplace UI 中，你会看到各种预构建操作。就像每个机器人都有特殊技能一样。有些擅长整理你的博客文章，有些擅长检查你的网站布局，等等。你正在寻找一个可以在你添加新博客文章时自动重建网站的操作。

![图 9.4 – GitHub Actions Marketplace 页面](image/B21847_09_4.jpg)
图 9.4 – GitHub Actions Marketplace 页面

以下是如何从 Marketplace 查找和使用操作的方法：
1.  搜索 **Actions**。
2.  前往 GitHub Marketplace (https://github.com/marketplace?type=actions)。
3.  使用搜索栏查找适合你需求的操作（例如，**deploy to GitHub Pages**），如 *图 9.4* 所示。
4.  复制使用代码片段。（Marketplace 中的每个操作都提供使用代码片段。）
5.  将此代码片段粘贴到你的工作流文件中。

Marketplace 中的每个操作都附带自己的说明和评论。就像阅读机器人包装盒背面以了解它能做什么，并查看在线评论以了解它对他人效果如何一样。
