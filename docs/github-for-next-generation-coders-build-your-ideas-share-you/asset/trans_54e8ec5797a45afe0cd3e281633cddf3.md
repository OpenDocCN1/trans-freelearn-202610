
让我们快速解释 Git 中 **cherry-picking** 的概念。它是一种用于跟踪文件更改并在多人之间协调工作的版本控制系统。图 7.32 进一步演示了这一点：

![图 7.32 – Git cherry-picking](image/B21847_07_32.jpg)

图 7.32 – Git cherry-picking

让我们详细解读前一幅图中显示的术语：

* **feature branch** 就像拥有一个单独的工作区或草稿本，开发者在其中绘制或编写新内容——在本例中，是您的一页式作品集网站的新功能。
* **main branch** 是每个人都能看到的网站最终稿。它是经过润色的版本，其中展示了所有完全完成的功能。

事件序列如下：

1. 开发者在其 feature branch 中工作，进行更改（称为 **commits**）。每个 commit 就像保存您网站设计的一个新版本。随着开发者进行更多更改，他们会保存更多 commits。他们处于一个 **loop** 中，不断进行更改并保存 commits，直到功能完成。
2. 在某个时候，开发者或其团队决定将 feature branch 中的一个特定更改（单个 commit）准备好添加到 main branch。
3. 开发者使用 `git switch main` 命令切换到 main branch。这就像说，“*好吧，让我们去 **最终稿。***”
4. 然后，他们使用 `git cherry-pick` 命令并指定来自 feature branch 的特定 commit。这就像说，“*让我们从我的草稿本中取出这个好主意并放入 **最终稿。***”
5. 被 cherry-pick 的 commit 应用到 main branch。这就像把草稿本上的贴纸粘到最终稿上。
6. main branch 现在有了新的更改，而 feature branch 仍然存在，其中包含所有可能仍在进行中的其他更改。

*loop* 表示开发过程是迭代的。开发者经常进行许多更改，并且可以在任何时候 cherry-pick 其中任何更改以添加到 main branch。

Cherry-picking 可用于从一系列 commits 中快速包含特定的修复或功能，而无需合并 feature branch 中的所有更改。这对于管理哪些内容准备好包含在您网站的 main branch 中非常有用。

## 摘要

恭喜您完成 [*第7章*](#B21847_07.xhtml#_idTextAnchor192)！您刚刚经历了 Git 中最强大的方面之一，学习如何控制和导航编码项目的历史。让我们花点时间回顾一下您掌握的关键技能和概念。

以下是您所取得的成果回顾。

* **理解 Git 的历史**：您深入研究了 `git log`，探索了查看项目时间线的各种方式。您已看到 `git log --oneline`、`git log --graph` 和 `git log --decorate` 如何将项目历史的理解从简单的列表转变为详细而信息丰富的地图。
* **git bisect – 侦探工具**：`git bisect` 命令成为您的侦探工具，帮助您高效地追踪引入 bug 的确切 commit。
* **回滚和重置**：您已学习使用 `git revert` 和 `git reset` 撤销更改的艺术。
* **使用 diff 探索更改**：`diff` 命令提供了在 commits、分支和工作目录之间比较更改的新方法。您获得了在细粒度级别上发现差异的能力，从而增强了对每次修改的洞察。
* **Cherry-picking – 选择性集成**：Cherry-picking 教会您从不同分支选择性地集成更改。

当您结束本章时，请记住您获得的技能不仅仅是管理代码——它们是关于管理时间和变化。

下一章将打开一座宝库的工具和命令，提升您的 Git 和 GitHub 体验。我们将涵盖一系列重要主题，例如 Sourcetree、GitHub Codespaces、GitHub Desktop、Visual Studio Code 和 DevHub 通知。

## 测验

请回答以下问题：

1. `git log` 在 Git 中做什么？
   1. 删除旧提交
   2. 显示提交历史
   3. 修改最新提交
   4. 创建新分支
   *Answer*: B. 显示提交历史。

2. 真或假：`git reflog` 命令可用于查看存储库中进行的操作（例如提交和合并）的列表。
   1. 真
   2. 假
   *Answer*: A. 真。

3. `git` ________ 是一个工具，通过自动测试提交来查找引入 bug 的提交。
   *Answer*: `bisect`.

4. 在 Git 中回滚提交的目的是什么？
   *Answer*: 回滚提交用于撤销特定提交所做的更改，将项目恢复到之前的状态，而不改变项目的历史记录。

5. `git diff` 命令显示什么？
   1. 分支之间的差异
   2. 提交之间的差异
   3. 工作目录中的当前更改
   4. 以上全部
   *Answer*: D. 以上全部。

6. 真或假：一旦提交被回滚，它就完全从项目的历史记录中删除。
   1. 真
   2. 假
   *Answer*: B. 假（提交仍在历史记录中，但其更改已被撤销）。

7. Git 中的 cherry-picking 是什么？
   1. 删除特定提交
   2. 从一个分支选择并应用特定提交到另一个分支
   3. 创建新存储库
   4. 重命名分支
   *Answer*: B. 从一个分支选择并应用特定提交到另一个分支。

8. 要撤销提交所做的更改并保留这些更改在工作目录中，您将使用 `git reset --`______。
   *Answer*: `soft`.

9. 真或假：`git bisect` 命令会自动修复它在代码中找到的 bug。
   1. 真
   2. 假
   *Answer*: B. 假（它有助于识别有问题的提交，但不会修复 bug）。

10. `git reset` 和 `git revert` 的主要区别是什么？
    *Answer*: `git reset` 命令用于通过将当前分支移动到特定提交来撤销更改，可能改变项目的历史记录，而 `git revert` 创建一个新的提交来撤销之前提交的更改，而不改变项目的历史记录。

## 增强挑战 – Space Explorer 启动屏幕、分数跟踪和掌握 Git 命令

在练习用于回滚、重置和 cherry-picking 的高级 Git 命令的同时，在 *Space Explorer* 中实现启动屏幕和分数跟踪：

![图 7.33 – Space Explorer 启动屏幕](image/B21847_07_33.jpg)

图 7.33 – Space Explorer 启动屏幕

* 这里是您更新的成功步骤 **：
1. **初始设置**：与之前一样，创建一个新分支 (**feature-splash-and-score**) 并开始实现启动屏幕和分数跟踪。
2. **故意“错误”**：在初始实现之后，在代码中做一些故意的“错误”或次优更改。提交这些更改。
3. **使用 git revert**：找出您犯的一个故意错误并使用 `git revert` 撤销该特定提交。这将帮助您了解如何回退已记录在 Git 历史记录中的更改。
4. **尝试 git reset**：对于另一个错误，使用 `git reset` 撤销更改。您可以尝试使用 `git reset --soft` 或 `git reset --hard`（请注意 `--hard` 会永久删除您的更改），以了解它们在处理项目历史记录方面的差异。
5. **Cherry-pick 一个功能**：创建一个新分支（例如，**additional-feature**）并为启动屏幕或计分系统添加一个小但明显的新的功能或增强。
