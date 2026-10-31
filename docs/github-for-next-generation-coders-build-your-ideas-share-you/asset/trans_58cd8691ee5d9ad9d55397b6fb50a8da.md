
提交后，切换回你的 `feature-splash-and-score` 分支，并使用 `git cherry-pick` 从 `additional-feature` 选择性地将新功能应用到你的主工作分支。

### 6. 最终测试和合并
彻底测试所有功能，包括启动画面功能和分数追踪，以确保一切无缝运行。
如果在合并到主分支时出现合并冲突，请使用你新获得的技能解决它们。

### 7. 反思与文档
1. 在 README 文件或提交信息中记录你的流程和挑战。
2. 反思使用这些 Git 命令如何帮助了你的开发流程。

你可以在以下链接下载代码：[https://github.com/PacktPublishing/GitHub-for-Next-Generation-Coders/tree/main/Space%20Explorer%20Game/Game%20Obsticles%20-%20Splash%20screen%20and%20player%20name](https://github.com/PacktPublishing/GitHub-for-Next-Generation-Coders/tree/main/Space%20Explorer%20Game/Game%20Obsticles%20-%20Splash%20screen%20and%20player%20name)。

# 8

## 有用的工具和 Git 命令
欢迎来到[第8章](#B21847_08.xhtml#_idTextAnchor213)，我们将深入 Git 和 GitHub 的世界，为你的单页作品集网站解锁新的效率和协作水平。你已经学习了基础知识，现在是时候发挥一些强大工具和命令的全部潜力了。
在本章中，我们将学习有用的 Git 命令，它们可以创建快捷方式、追踪文件的演变、重新排列提交并执行更多此类任务。然后我们将介绍 Sourcetree 以及它如何将 Git 的复杂性转变为项目旅程的可视化地图。我们将学习如何使用 Codespaces 在任何地方编码，使用 GitHub Desktop 管理存储库，并设置 DevHub，一个个人桌面助手，让你随时了解 GitHub 项目中的所有活动。
这里是我们将探索内容的预览：
* 打造快捷方式并保持整洁 – 高级 Git 命令
* 使用 Sourcetree 简化你的网站工作
* 使用 GitHub Desktop 导航你的网站项目
* 使用 GitHub Codespaces 打造你的网站
* 使用 DevHub 管理你的项目的动态

### 打造快捷方式并保持整洁 – 高级 Git 命令
在构建网站时，你可能会发现自己重复使用一些冗长的 Git 命令。拥有快捷方式不是更容易吗？或者只需一个命令就能清理不必要的文件？本节将介绍一些可以做到这一点甚至更多的高级命令。
在你的单页作品集网站上工作有时会感觉像在杂耍。你有想尝试的新想法，你正在修复错误，同时还要跟踪所有更改。以下是一些 Git 命令，它们就像巧妙的工具，帮助你更高效地杂耍。
* 使用 git alias 创建自己的快捷方式：使用 `git config --global alias.[alias_name] [git_command]` 为你经常使用的命令创建快捷方式。例如，如果你经常检查状态，可以设置一个别名，例如 `git config --global alias.st status`。
现在，不必每次都输入 `git status`，你只需输入 `git st`：
![图 8.1 – Git 别名状态](image/B21847_08_1.jpg)
图 8.1 – Git 别名状态
* 使用 git log follow 跨更改追踪文件历史：当你想查看特定文件随时间所做的更改时，使用 `git log --follow [file]`。它就像为你网站代码的某一部分提供详细的 timeline。
例如，让我们查看 `index.html` 文件的日志并输入 `git log --follow index.html`。你将得到 `index.html` 文件的详细历史，如图 8.2 所示：
![图 8.2 – git log follow](image/B21847_08_2.jpg)
图 8.2 – git log follow
* 使用 git rebase 重新排列你的工作：`git rebase [branch]` 命令允许你将一系列提交移动或合并到新的基础提交。它有点像重新排列你网站故事中的页面，以使叙述更流畅。
* 使用 git clean 清理未跟踪文件：要从工作目录中删除未跟踪文件，`git clean -fd` 是你的首选命令。把它想象成通过删除所有未进入你网站的纸屑和未使用的草图来保持工作空间整洁。
* 使用 git stash 保存进行中的工作：想象一下，你正在为网站制作新设计，突然有人要求你添加新功能。你需要一种快速隐藏设计并清理本地仓库而不丢失已完成工作的方法。`git stash` 就像那个保存选项；它会获取所有更改并暂时隐藏它们，让你拥有一个干净的工作目录。
* 使用 git stash pop 恢复已暂存的更改：在功能实现后你想回到设计时，`git stash pop` 会移除覆盖层并恢复你的工作。它就像揭开你的 CSS 设计和图像，立即回到你离开的地方。
* 使用 git tag 标记里程碑：当你在网站开发中达到重要节点，例如完成关于部分时，你可能想标记它。`git tag [tagname]` 就像在时间线上那个点上贴一张便签，以便以后能轻松找到它。
* 使用 git blame 找出谁做了更改：如果注意到网站上的某些内容发生了变化而你不确定是谁做的，`git blame [file]` 会显示每个文件行最后由谁编辑以及何时编辑。它就像做侦探工作来弄清楚谁移动或删除了你的设计。
例如，让我们尝试 `index.html` 文件看看会得到什么结果。输入 `git blame index.html` 后，你将看到整个文件以及每一行和做出增添或更改的人员姓名：
![图 8.3 – git blame 结果](image/B21847_08_3.jpg)
图 8.3 – git blame 结果
* 使用 git fetch --all 从所有分支获取更新：`git fetch --all` 命令就像检查所有邮箱是否有消息。它获取 GitHub 仓库中所有分支的最新更新，而不会将它们合并到你的工作中。你可以在决定将什么合并到自己的分支之前，查看其他人都在做什么。
* 使用 git remote 检查你的远程仓库：要查看你对仓库其他版本的远程连接列表，`git remote -v` 就像拥有一本地址簿。它显示了你可以推送更改或获取的位置。
这些高级命令为管理你的网站项目提供了更大的灵活性和效率。它们就像在你的工作坊中拥有一套电动工具，使你的工作更整洁，你的努力更高效。
它们确保你可以保存工作、跟踪贡献、标记重要里程碑并随时了解每个人的工作。
一切都是为了让你的网站开发过程尽可能顺畅无压力！在下一节中，我们将学习更多可以帮助我们管理 Git 和仓库的工具。

### 使用 Sourcetree 简化你的网站工作
在你创建单页作品集网站时，而不是使用命令行 Git 命令手动处理所有更新和更改，你有一个工具可以直观地为你展示一切。该工具是 Atlassian 的 Sourcetree。
![图 8.4 – Sourcetree](image/B21847_08_4.jpg)
图 8.4 – Sourcetree

#### 为什么应该使用 Sourcetree？
Sourcetree 以易于理解的方式呈现你的项目。它就像拥有你所有工作的地图，从大局到细节。
如果输入 Git 命令感觉像在外语中点咖啡，Sourcetree 就像有一位友好的咖啡师知道你的常规订单。只需指向并点击即可。
