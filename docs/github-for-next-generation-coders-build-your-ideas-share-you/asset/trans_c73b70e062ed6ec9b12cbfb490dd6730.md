
你已经完成了你的单页作品集网站，并且刚刚完成了新的左侧菜单并为其添加了链接和社交图标。你在本地的 `feature/leftside-menu` 分支中完成了修改。现在，你希望这些更改能在 GitHub 上可用，以便他人查看或贡献。这就是 `git push` 发挥作用的地方。

通常，你只需输入 `git push`，这会告诉 Git 你想要将提交从本地仓库传输到远程仓库（如 GitHub）。

在这种情况下，你会收到一条错误信息，提示你无法推送到远程仓库，因为你在本地创建的分支在 GitHub 仓库中仍然不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

如你所见，该消息还提供了如何使用 `--set-upstream` 将本地功能分支推送到远程仓库并设置上游的说明。这就像为未来的推送设置默认路径。你是在告诉 Git，“*嘿，下次我从此分支推送更改时，我希望它们去同一个地方，而无需我再次指定。*”

命令的下一部分是 `origin`。这是 Git 为你克隆的远程仓库默认命名的名称。在大多数情况下，`origin` 只是你 GitHub 仓库 URL 的简写，而 `feature/leftside-menu` 是你的分支名称。你是在指定想要推送此分支的提交。

## 运行此命令时会发生什么？

通过使用 `--set-upstream`，你是在本地分支 `feature/leftside-menu` 与 GitHub 上的分支（`origin`）之间建立链接。如果该分支在 GitHub 上尚不存在，此命令将为你创建它。

Git 会将你在 `feature/leftside-menu` 分支中所做的所有提交上传到 GitHub，并将其放在同名分支中。

一旦你设置过上游，你就可以在 `feature/leftside-menu` 分支中使用 `git push`，Git 就知道将更改发送到哪里。那么，让我们在终端中运行此命令，看看会发生什么：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

通过将分支推送到 GitHub，你让团队成员可以看到你的新左侧菜单、提供反馈或添加自己的调整。

如你所见，我们的新功能分支已在 GitHub 中创建，并且所有对文件的更改都已随分支一起推送。GitHub 已为我们提供了一条消息，提示访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu。这就是我们应该创建 PR 的地方。

现在，你可以创建 PR 将其合并到主项目中，将你时尚的左侧菜单集成到主要的作品集网站中。

## 这些区域为什么重要？

这些区域帮助你组织工作。你可以在工作区中进行实验，在暂存区决定喜欢哪些更改，并在历史记录中保存最佳工作。这种分离让你能够控制项目的演进。你可以进行更改、尝试新事物，并且只提交你确定的那些。如果出现问题，你始终可以回到历史记录中的先前状态。

# 摘要

本章提供了关于编码中团队合作和创造力的[content]
你已经完成了你的单页作品集网站，并且刚刚完成了新的左侧菜单并为其添加了链接和社交图标。你在本地的 `feature/leftside-menu` 分支中完成了修改。现在，你希望这些更改能在 GitHub 上可用，以便他人查看或贡献。这就是 `git push` 发挥作用的地方。

通常，你只需输入 `git push`，这会告诉 Git 你想要将提交从本地仓库传输到远程仓库（如 GitHub）。

在这种情况下，你会收到一条错误信息，提示你无法推送到远程仓库，因为你在本地创建的分支在 GitHub 仓库中仍然不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

如你所见，该消息还提供了如何使用 `--set-upstream` 将本地功能分支推送到远程仓库并设置上游的说明。这就像为未来的推送设置默认路径。你是在告诉 Git，“*嘿，下次我从此分支推送更改时，我希望它们去同一个地方，而无需我再次指定。*”

命令的下一部分是 `origin`。这是 Git 为你克隆的远程仓库默认命名的名称。在大多数情况下，`origin` 只是你 GitHub 仓库 URL 的简写，而 `feature/leftside-menu` 是你的分支名称。你是在指定想要推送此分支的提交。

## 运行此命令时会发生什么？

通过使用 `--set-upstream`，你是在本地分支 `feature/leftside-menu` 与 GitHub 上的分支（`origin`）之间建立链接。如果该分支在 GitHub 上尚不存在，此命令将为你创建它。

Git 会将你在 `feature/leftside-menu` 分支中所做的所有提交上传到 GitHub，并将其放在同名分支中。

一旦你设置过上游，你就可以在 `feature/leftside-menu` 分支中使用 `git push`，Git 就知道将更改发送到哪里。那么，让我们在终端中运行此命令，看看会发生什么：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

通过将分支推送到 GitHub，你让团队成员可以看到你的新左侧菜单、提供反馈或添加自己的调整。

如你所见，我们的新功能分支已在 GitHub 中创建，并且所有对文件的更改都已随分支一起推送。GitHub 已为我们提供了一条消息，提示访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu。这就是我们应该创建 PR 的地方。

现在，你可以创建 PR 将其合并到主项目中，将你时尚的左侧菜单集成到主要的作品集网站中。

## 这些区域为什么重要？

这些区域帮助你组织工作。你可以在工作区中进行实验，在暂存区决定喜欢哪些更改，并在历史记录中保存最佳工作。这种分离让你能够控制项目的演进。你可以进行更改、尝试新事物，并且只提交你确定的那些。如果出现问题，你始终可以回到历史记录中的先前状态。

# 摘要

本章提供了关于编码中团队合作和创造力的[content]
你已经完成了你的单页作品集网站，并且刚刚完成了新的左侧菜单并为其添加了链接和社交图标。你在本地的 `feature/leftside-menu` 分支中完成了修改。现在，你希望这些更改能在 GitHub 上可用，以便他人查看或贡献。这就是 `git push` 发挥作用的地方。

通常，你只需输入 `git push`，这会告诉 Git 你想要将提交从本地仓库传输到远程仓库（如 GitHub）。

在这种情况下，你会收到一条错误信息，提示你无法推送到远程仓库，因为你在本地创建的分支在 GitHub 仓库中仍然不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

如你所见，该消息还提供了如何使用 `--set-upstream` 将本地功能分支推送到远程仓库并设置上游的说明。这就像为未来的推送设置默认路径。你是在告诉 Git，“*嘿，下次我从此分支推送更改时，我希望它们去同一个地方，而无需我再次指定。*”

命令的下一部分是 `origin`。这是 Git 为你克隆的远程仓库默认命名的名称。在大多数情况下，`origin` 只是你 GitHub 仓库 URL 的简写，而 `feature/leftside-menu` 是你的分支名称。你是在指定想要推送此分支的提交。

## 运行此命令时会发生什么？

通过使用 `--set-upstream`，你是在本地分支 `feature/leftside-menu` 与 GitHub 上的分支（`origin`）之间建立链接。如果该分支在 GitHub 上尚不存在，此命令将为你创建它。

Git 会将你在 `feature/leftside-menu` 分支中所做的所有提交上传到 GitHub，并将其放在同名分支中。

一旦你设置过上游，你就可以在 `feature/leftside-menu` 分支中使用 `git push`，Git 就知道将更改发送到哪里。那么，让我们在终端中运行此命令，看看会发生什么：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

通过将分支推送到 GitHub，你让团队成员可以看到你的新左侧菜单、提供反馈或添加自己的调整。

如你所见，我们的新功能分支已在 GitHub 中创建，并且所有对文件的更改都已随分支一起推送。GitHub 已为我们提供了一条消息，提示访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu。这就是我们应该创建 PR 的地方。

现在，你可以创建 PR 将其合并到主项目中，将你时尚的左侧菜单集成到主要的作品集网站中。

## 这些区域为什么重要？

这些区域帮助你组织工作。你可以在工作区中进行实验，在暂存区决定喜欢哪些更改，并在历史记录中保存最佳工作。这种分离让你能够控制项目的演进。你可以进行更改、尝试新事物，并且只提交你确定的那些。如果出现问题，你始终可以回到历史记录中的先前状态。

# 摘要

本章提供了关于编码中团队合作和创造力的[content]
你已经完成了你的单页作品集网站，并且刚刚完成了新的左侧菜单并为其添加了链接和社交图标。你在本地的 `feature/leftside-menu` 分支中完成了修改。现在，你希望这些更改能在 GitHub 上可用，以便他人查看或贡献。这就是 `git push` 发挥作用的地方。

通常，你只需输入 `git push`，这会告诉 Git 你想要将提交从本地仓库传输到远程仓库（如 GitHub）。

在这种情况下，你会收到一条错误信息，提示你无法推送到远程仓库，因为你在本地创建的分支在 GitHub 仓库中仍然不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

如你所见，该消息还提供了如何使用 `--set-upstream` 将本地功能分支推送到远程仓库并设置上游的说明。这就像为未来的推送设置默认路径。你是在告诉 Git，“*嘿，下次我从此分支推送更改时，我希望它们去同一个地方，而无需我再次指定。*”

命令的下一部分是 `origin`。这是 Git 为你克隆的远程仓库默认命名的名称。在大多数情况下，`origin` 只是你 GitHub 仓库 URL 的简写，而 `feature/leftside-menu` 是你的分支名称。你是在指定想要推送此分支的提交。

## 运行此命令时会发生什么？

通过使用 `--set-upstream`，你是在本地分支 `feature/leftside-menu` 与 GitHub 上的分支（`origin`）之间建立链接。如果该分支在 GitHub 上尚不存在，此命令将为你创建它。

Git 会将你在 `feature/leftside-menu` 分支中所做的所有提交上传到 GitHub，并将其放在同名分支中。

一旦你设置过上游，你就可以在 `feature/leftside-menu` 分支中使用 `git push`，Git 就知道将更改发送到哪里。那么，让我们在终端中运行此命令，看看会发生什么：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

通过将分支推送到 GitHub，你让团队成员可以看到你的新左侧菜单、提供反馈或添加自己的调整。

如你所见，我们的新功能分支已在 GitHub 中创建，并且所有对文件的更改都已随分支一起推送。GitHub 已为我们提供了一条消息，提示访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu。这就是我们应该创建 PR 的地方。

现在，你可以创建 PR 将其合并到主项目中，将你时尚的左侧菜单集成到主要的作品集网站中。

## 这些区域为什么重要？

这些区域帮助你组织工作。你可以在工作区中进行实验，在暂存区决定喜欢哪些更改，并在历史记录中保存最佳工作。这种分离让你能够控制项目的演进。你可以进行更改、尝试新事物，并且只提交你确定的那些。如果出现问题，你始终可以回到历史记录中的先前状态。

# 摘要

本章提供了关于编码中团队合作和创造力的[content]
你已经完成了你的单页作品集网站，并且刚刚完成了新的左侧菜单并为其添加了链接和社交图标。你在本地的 `feature/leftside-menu` 分支中完成了修改。现在，你希望这些更改能在 GitHub 上可用，以便他人查看或贡献。这就是 `git push` 发挥作用的地方。

通常，你只需输入 `git push`，这会告诉 Git 你想要将提交从本地仓库传输到远程仓库（如 GitHub）。

在这种情况下，你会收到一条错误信息，提示你无法推送到远程仓库，因为你在本地创建的分支在 GitHub 仓库中仍然不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

如你所见，该消息还提供了如何使用 `--set-upstream` 将本地功能分支推送到远程仓库并设置上游的说明。这就像为未来的推送设置默认路径。你是在告诉 Git，“*嘿，下次我从此分支推送更改时，我希望它们去同一个地方，而无需我再次指定。*”

命令的下一部分是 `origin`。这是 Git 为你克隆的远程仓库默认命名的名称。在大多数情况下，`origin` 只是你 GitHub 仓库 URL 的简写，而 `feature/leftside-menu` 是你的分支名称。你是在指定想要推送此分支的提交。

## 运行此命令时会发生什么？

通过使用 `--set-upstream`，你是在本地分支 `feature/leftside-menu` 与 GitHub 上的分支（`origin`）之间建立链接。如果该分支在 GitHub 上尚不存在，此命令将为你创建它。

Git 会将你在 `feature/leftside-menu` 分支中所做的所有提交上传到 GitHub，并将其放在同名分支中。

一旦你设置过上游，你就可以在 `feature/leftside-menu` 分支中使用 `git push`，Git 就知道将更改发送到哪里。那么，让我们在终端中运行此命令，看看会发生什么：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

通过将分支推送到 GitHub，你让团队成员可以看到你的新左侧菜单、提供反馈或添加自己的调整。

如你所见，我们的新功能分支已在 GitHub 中创建，并且所有对文件的更改都已随分支一起推送。GitHub 已为我们提供了一条消息，提示访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu。这就是我们应该创建 PR 的地方。

现在，你可以创建 PR 将其合并到主项目中，将你时尚的左侧菜单集成到主要的作品集网站中。

## 这些区域为什么重要？

这些区域帮助你组织工作。你可以在工作区中进行实验，在暂存区决定喜欢哪些更改，并在历史记录中保存最佳工作。这种分离让你能够控制项目的演进。你可以进行更改、尝试新事物，并且只提交你确定的那些。如果出现问题，你始终可以回到历史记录中的先前状态。

# 摘要

本章提供了关于编码中团队合作和创造力的[content]
你已经完成了你的单页作品集网站，并且刚刚完成了新的左侧菜单并为其添加了链接和社交图标。你在本地的 `feature/leftside-menu` 分支中完成了修改。现在，你希望这些更改能在 GitHub 上可用，以便他人查看或贡献。这就是 `git push` 发挥作用的地方。

通常，你只需输入 `git push`，这会告诉 Git 你想要将提交从本地仓库传输到远程仓库（如 GitHub）。

在这种情况下，你会收到一条错误信息，提示你无法推送到远程仓库，因为你在本地创建的分支在 GitHub 仓库中仍然不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

如你所见，该消息还提供了如何使用 `--set-upstream` 将本地功能分支推送到远程仓库并设置上游的说明。这就像为未来的推送设置默认路径。你是在告诉 Git，“*嘿，下次我从此分支推送更改时，我希望它们去同一个地方，而无需我再次指定。*”

命令的下一部分是 `origin`。这是 Git 为你克隆的远程仓库默认命名的名称。在大多数情况下，`origin` 只是你 GitHub 仓库 URL 的简写，而 `feature/leftside-menu` 是你的分支名称。你是在指定想要推送此分支的提交。

## 运行此命令时会发生什么？

通过使用 `--set-upstream`，你是在本地分支 `feature/leftside-menu` 与 GitHub 上的分支（`origin`）之间建立链接。如果该分支在 GitHub 上尚不存在，此命令将为你创建它。

Git 会将你在 `feature/leftside-menu` 分支中所做的所有提交上传到 GitHub，并将其放在同名分支中。

一旦你设置过上游，你就可以在 `feature/leftside-menu` 分支中使用 `git push`，Git 就知道将更改发送到哪里。那么，让我们在终端中运行此命令，看看会发生什么：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

通过将分支推送到 GitHub，你让团队成员可以看到你的新左侧菜单、提供反馈或添加自己的调整。

如你所见，我们的新功能分支已在 GitHub 中创建，并且所有对文件的更改都已随分支一起推送。GitHub 已为我们提供了一条消息，提示访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu。这就是我们应该创建 PR 的地方。

现在，你可以创建 PR 将其合并到主项目中，将你时尚的左侧菜单集成到主要的作品集网站中。

## 这些区域为什么重要？

这些区域帮助你组织工作。你可以在工作区中进行实验，在暂存区决定喜欢哪些更改，并在历史记录中保存最佳工作。这种分离让你能够控制项目的演进。你可以进行更改、尝试新事物，并且只提交你确定的那些。如果出现问题，你始终可以回到历史记录中的先前状态。

# 摘要

本章提供了关于编码中团队合作和创造力的[content]
你已完成单页作品集网站，并完成了新的左侧菜单并添加了链接和社交图标。你在本地 `feature/leftside-menu` 分支中做了修改。现在希望这些更改在 GitHub 上可用，让他人查看或贡献。这就是 `git push` 发挥作用的地方。

通常只需输入 `git push`，告诉 Git 将本地仓库的提交传输到远程仓库（如 GitHub）。

此情况下会收到错误，提示无法推送到远程仓库，因为本地创建的分支在 GitHub 仓库中仍不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

如你所见，消息还说明了如何使用 `--set-upstream` 将本地功能分支推送到远程仓库并设置上游。这相当于为未来推送设置默认路径。你在告诉 Git，“*嘿，下次从此分支推送更改时，我希望它们去同一个地方，无需再次指定。*”

命令的下一部分是 `origin`。这是 Git 给克隆的远程仓库的默认名称。通常 `origin` 只是 GitHub 仓库 URL 的简写，`feature/leftside-menu` 是分支名。你指定要推送此分支的提交。

## 运行此命令时会发生什么？

使用 `--set-upstream` 会在本地分支 `feature/leftside-menu` 与 GitHub 上的分支 `origin` 之间建立链接。若该分支在 GitHub 上尚不存在，此命令会为你创建它。

Git 会将你在 `feature/leftside-menu` 分支做的所有提交上传到 GitHub，放在同名分支中。

设置一次上游后，在 `feature/leftside-menu` 分支中使用 `git push` 即可，Git 会知道发送到哪里。让我们在终端运行此命令看看：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

推送分支到 GitHub 后，团队成员可看到新的左侧菜单、提供反馈或添加调整。

如你所见，新功能分支已在 GitHub 创建，所有文件更改已随分支推送。GitHub 提供消息访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu，这里应创建 PR。

现在可创建 PR 合并到主项目，将时尚的左侧菜单集成到主作品集网站。

## 这些区域为什么重要？

这些区域帮助组织工作。可在工作区实验，在暂存区决定喜欢的更改，在历史记录保存最佳工作。这种分离让你控制项目演进。可更改、尝试新事物，只提交确定的。若出错，可随时回到历史记录的先前状态。

# 摘要

本章提供了编码团队合作与创造力的[content]
你已完成单页作品集网站，并完成了新的左侧菜单并添加了链接和社交图标。你在本地 `feature/leftside-menu` 分支中做了修改。现在希望这些更改在 GitHub 上可用，让他人查看或贡献。这就是 `git push` 发挥作用的地方。

通常只需输入 `git push`，告诉 Git 将本地仓库的提交传输到远程仓库（如 GitHub）。

此情况下会收到错误，提示无法推送到远程仓库，因为本地创建的分支在 GitHub 仓库中仍不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

如你所见，消息还说明了如何使用 `--set-upstream` 将本地功能分支推送到远程仓库并设置上游。这相当于为未来推送设置默认路径。你在告诉 Git，“*嘿，下次从此分支推送更改时，我希望它们去同一个地方，无需再次指定。*”

命令的下一部分是 `origin`。这是 Git 给克隆的远程仓库的默认名称。通常 `origin` 只是 GitHub 仓库 URL 的简写，`feature/leftside-menu` 是分支名。你指定要推送此分支的提交。

## 运行此命令时会发生什么？

使用 `--set-upstream` 会在本地分支 `feature/leftside-menu` 与 GitHub 上的分支 `origin` 之间建立链接。若该分支在 GitHub 上尚不存在，此命令会为你创建它。

Git 会将你在 `feature/leftside-menu` 分支做的所有提交上传到 GitHub，放在同名分支中。

设置一次上游后，在 `feature/leftside-menu` 分支中使用 `git push` 即可，Git 会知道发送到哪里。让我们在终端运行此命令看看：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

推送分支到 GitHub 后，团队成员可看到新的左侧菜单、提供反馈或添加调整。

如你所见，新功能分支已在 GitHub 创建，所有文件更改已随分支推送。GitHub 提供消息访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu，这里应创建 PR。

现在可创建 PR 合并到主项目，将时尚的左侧菜单集成到主作品集网站。

## 这些区域为什么重要？

这些区域帮助组织工作。可在工作区实验，在暂存区决定喜欢的更改，在历史记录保存最佳工作。这种分离让你控制项目演进。可更改、尝试新事物，只提交确定的。若出错，可随时回到历史记录的先前状态。

# 摘要

本章是关于编码协作与创造力的[content]
你已完成单页作品集网站，并完成了新的左侧菜单并添加了链接和社交图标。你在本地 `feature/leftside-menu` 分支中做了修改。现在希望这些更改在 GitHub 上可用，让他人查看或贡献。这就是 `git push` 发挥作用的地方。

通常只需输入 `git push`，告诉 Git 将本地仓库的提交传输到远程仓库（如 GitHub）。

此情况下会收到错误，提示无法推送到远程仓库，因为本地创建的分支在 GitHub 仓库中仍不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

如你所见，消息还说明了如何使用 `--set-upstream` 将本地功能分支推送到远程仓库并设置上游。这相当于为未来推送设置默认路径。你在告诉 Git，“*嘿，下次从此分支推送更改时，我希望它们去同一个地方，无需再次指定。*”

命令的下一部分是 `origin`。这是 Git 给克隆的远程仓库的默认名称。通常 `origin` 只是 GitHub 仓库 URL 的简写，`feature/leftside-menu` 是分支名。你指定要推送此分支的提交。

## 运行此命令时会发生什么？

使用 `--set-upstream` 会在本地分支 `feature/leftside-menu` 与 GitHub 上的分支 `origin` 之间建立链接。若该分支在 GitHub 上尚不存在，此命令会为你创建它。

Git 会将你在 `feature/leftside-menu` 分支做的所有提交上传到 GitHub，放在同名分支中。

设置一次上游后，在 `feature/leftside-menu` 分支中使用 `git push` 即可，Git 会知道发送到哪里。让我们在终端运行此命令看看：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

推送分支到 GitHub 后，团队成员可看到新的左侧菜单、提供反馈或添加调整。

如你所见，新功能分支已在 GitHub 创建，所有文件更改已随分支推送。GitHub 提供消息访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu，这里应创建 PR。

现在可创建 PR 合并到主项目，将时尚的左侧菜单集成到主作品集网站。

## 这些区域为什么重要？

这些区域帮助组织工作。可在工作区实验，在暂存区决定喜欢的更改，在历史记录保存最佳工作。这种分离让你控制项目演进。可更改、尝试新事物，只提交确定的。若出错，可随时回到历史记录的先前状态。

# 摘要

本章是关于编码协作与创造力的[content]
你已完成单页作品集网站，并完成新的左侧菜单并添加链接和社交图标。你在本地 `feature/leftside-menu` 分支做了修改。现在希望更改在 GitHub 上可用，让他人查看或贡献。这就是 `git push` 的作用。

通常只需输入 `git push`，告诉 Git 将本地提交传输到远程仓库如 GitHub。

此时会收到错误，提示无法推送到远程，因为本地创建的分支在 GitHub 仓库中不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

消息还说明如何用 `--set-upstream` 将本地功能分支推送到远程并设置上游。这相当于为未来推送设默认路径。你在告诉 Git，“*嘿，下次从此分支推送更改时，我希望它们去同一个地方，无需再次指定。*”

命令下一部分是 `origin`。这是 Git 给克隆远程仓库的默认名。通常 `origin` 只是 GitHub 仓库 URL 的简写，`feature/leftside-menu` 是分支名。你指定要推送此分支的提交。

## 运行此命令时会发生什么？

用 `--set-upstream` 会在本地分支 `feature/leftside-menu` 与 GitHub 分支 `origin` 间建立链接。若该分支在 GitHub 不存在，此命令会创建它。

Git 会将你在 `feature/leftside-menu` 分支的所有提交上传到 GitHub，放在同名分支。

设置一次上游后，在该分支用 `git push` 即可，Git 知道去向。让我们在终端运行看看：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

推送分支到 GitHub 后，团队成员可看到新左侧菜单、提供反馈或添加调整。

如你所见，新功能分支已在 GitHub 创建，所有文件更改已随分支推送。GitHub 提供消息访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu，这里应创建 PR。

现在可创建 PR 合并到主项目，将时尚的左侧菜单集成到主作品集网站。

## 这些区域为什么重要？

这些区域帮助组织工作。可在工作区实验，在暂存区决定喜欢的更改，在历史记录保存最佳工作。这种分离让你控制项目演进。可更改、尝试新事物，只提交确定的。若出错，可随时回到历史记录的先前状态。

# 摘要

本章是编码协作与创造力的[content]
你已完成单页作品集网站，并完成新的左侧菜单并添加链接和社交图标。你在本地 `feature/leftside-menu` 分支做了修改。现在希望更改在 GitHub 上可用，让他人查看或贡献。这就是 `git push` 的作用。

通常只需输入 `git push`，告诉 Git 将本地提交传输到远程仓库如 GitHub。

此时会收到错误，提示无法推送到远程，因为本地创建的分支在 GitHub 仓库中不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

消息还说明如何用 `--set-upstream` 将本地功能分支推送到远程并设置上游。这相当于为未来推送设默认路径。你在告诉 Git，“*嘿，下次从此分支推送更改时，我希望它们去同一个地方，无需再次指定。*”

命令下一部分是 `origin`。这是 Git 给克隆远程仓库的默认名。通常 `origin` 只是 GitHub 仓库 URL 的简写，`feature/leftside-menu` 是分支名。你指定要推送此分支的提交。

## 运行此命令时会发生什么？

用 `--set-upstream` 会在本地分支 `feature/leftside-menu` 与 GitHub 分支 `origin` 间建立链接。若该分支在 GitHub 不存在，此命令会创建它。

Git 会将你在 `feature/leftside-menu` 分支的所有提交上传到 GitHub，放在同名分支。

设置一次上游后，在该分支用 `git push` 即可，Git 知道去向。让我们在终端运行看看：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

推送分支到 GitHub 后，团队成员可看到新左侧菜单、提供反馈或添加调整。

如你所见，新功能分支已在 GitHub 创建，所有文件更改已随分支推送。GitHub 提供消息访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu，这里应创建 PR。

现在可创建 PR 合并到主项目，将时尚的左侧菜单集成到主作品集网站。

## 这些区域为什么重要？

这些区域帮助组织工作。可在工作区实验，在暂存区决定喜欢的更改，在历史记录保存最佳工作。这种分离让你控制项目演进。可更改、尝试新事物，只提交确定的。若出错，可随时回到历史记录的先前状态。

# 摘要

本章是编码协作与创造力的[content]
你已完成单页作品集网站，并完成新的左侧菜单并添加链接和社交图标。你在本地 `feature/leftside-menu` 分支做了修改。现在希望更改在 GitHub 上可用，让他人查看或贡献。这就是 `git push` 的作用。

通常只需输入 `git push`，告诉 Git 将本地提交传输到远程仓库如 GitHub。

此时会收到错误，提示无法推送到远程，因为本地创建的分支在 GitHub 仓库中不存在：

![图 4.28 – 运行 git push 命令](image/B21847_04_28.jpg)
图 4.28 – 运行 git push 命令

消息还说明如何用 `--set-upstream` 将本地功能分支推送到远程并设置上游。这相当于为未来推送设默认路径。你在告诉 Git，“*嘿，下次从此分支推送更改时，我希望它们去同一个地方，无需再次指定。*”

命令下一部分是 `origin`。这是 Git 给克隆远程仓库的默认名。通常 `origin` 只是 GitHub 仓库 URL 的简写，`feature/leftside-menu` 是分支名。你指定要推送此分支的提交。

## 运行此命令时会发生什么？

用 `--set-upstream` 会在本地分支 `feature/leftside-menu` 与 GitHub 分支 `origin` 间建立链接。若该分支在 GitHub 不存在，此命令会创建它。

Git 会将你在 `feature/leftside-menu` 分支的所有提交上传到 GitHub，放在同名分支。

设置一次上游后，在该分支用 `git push` 即可，Git 知道去向。让我们在终端运行看看：

![图 4.29 – 推送到远程的 Git](image/B21847_04_29.jpg)
图 4.29 – 推送到远程的 Git

推送分支到 GitHub 后，团队成员可看到新左侧菜单、提供反馈或添加调整。

如你所见，新功能分支已在 GitHub 创建，所有文件更改已随分支推送。GitHub 提供消息访问 <YOUR-REPOSITORY-URL>/MyFirstRepo/pull/new/feature/leftside-menu，这里应创建 PR。

现在可创建 PR 合并到主项目，将时尚的左侧菜单集成到主作品集网站。

## 这些区域为什么重要？

这些区域帮助组织工作。可在工作区实验，在暂存区决定喜欢的更改，在历史记录保存最佳工作。这种分离让你控制项目演进。可更改、尝试新事物，只提交确定的。若出错，可随时回到历史记录的先前状态。

# 摘要

本章展示了编码团队合作与创造力的精髓。你现在知道如何在项目中创建分支。就像拥有个人实验室，可在不影响主项目的情况下实验。你还学会了为工作创建检查点。每次做出酷炫更改或添加新内容时，都将其保存为检查点。最后，你学会了将更改发送到 GitHub 画廊，团队可看到你的进展。

下一章你将学习如何展示工作并请求反馈。你将学习让 PR 脱颖而出的秘诀，如何将其连接到项目任务以及如何审查 PR。就像成为评委团的一员，帮助决定哪些内容进入项目。

# 测验

请回答以下问题：

1. GitHub 中的 *branching* 是什么？
    1. 剪掉项目的一部分
    2. 创建项目的副本以单独工作
    3. 将两个项目合并为一个
    4. 删除旧项目文件
    *Answer*: B. Creating a copy of your project to work on separately.

2. 真或假：分支仅由高级程序员使用。
    1. 真
    2. 假
    *Answer*: B. False.

3. 当你想在不影响主项目的情况下为项目添加新功能时，你在 GitHub 中创建一个新的 ________。
    *Answer*: branch.

4. 在协作项目中分支有什么用？
    *Answer*: Branching allows multiple people to work on different parts of the project simultaneously without interfering with each other’s work.

5. 在 GitHub 项目中你可以用分支做什么？
    1. 与朋友分享
    2. 合并回主项目
    3. 使用后删除
    4. 以上全部
    *Answer*: D. All of the above.

6. 真或假：GitHub 中的每个分支都必须合并回主项目。
    1. 真
    2. 假
    *Answer*: B. False.

7. GitHub 中分支的常见用途是什么？
    1. 修复错误
    2. 尝试新想法
    3. 添加新功能
    4. 以上全部
    *Answer*: D. All of the above.

8. GitHub 中的分支允许你在“安全空间”中进行更改，而不影响 ________ 分支。
    *Answer*: `main`.

9. 真或假：一旦在 GitHub 中创建分支，它就成为新的主项目。
    1. 真
    2. 假
    *Answer*: B. False.

10. 你在分支上完成工作后该怎么做？
    *Answer*: Once you’ve finished work on a branch, you should consider merging it back into the main project or sharing it for feedback.

# 挑战 – 在 Space Explorer 中导航星辰！

你好，年轻的太空探险家！在本章中，你学习了 GitHub 用于协作项目的一些很酷的功能。在这里，你将应用这些技能，使用 GitHub 协作工具为你的 *Space Explorer* 游戏添加飞行障碍和星星。你准备好迎接这场星空挑战了吗？

## 你的任务？

![图 4.30 – Space Explorer 游戏画面](image/B21847_04_30.jpg)
图 4.30 – Space Explorer 游戏画面

使用 GitHub 协作工具将新功能 – 碰撞检测和计分 – 集成到你的 *Space Explorer* 游戏中。

**碰撞检测**: https://github.com/PacktPublishing/GitHub-for-Next-Generation-Coders/tree/main/Space%20Explorer%20Game/Game%20Obsticles%20-%20Colision%20detection

**计分**: https://github.com/PacktPublishing/GitHub-for-Next-Generation-Coders/tree/main/Space%20Explorer%20Game/Game%20Obsticles%20-%20Scoring

## 成功步骤

按以下步骤操作：

1. **创建功能分支**：在你的 *Space Explorer* 游戏的 GitHub 仓库中，创建一个名为 `feature-stars-and-obstacles-collision` 的新分支。此分支是你的个人航天器，你将在其中为游戏添加 dazzling 新功能，而不影响主代码库。

2. **拉取新功能**：我会为你提供飞行障碍和星星碰撞的代码。使用 `git pull` 命令将这些新功能带入你本地的 `feature-stars-and-obstacles` 分支副本。

3. **探索和集成**：将新代码合并到游戏中。调整并尝试。如何改变星星的颜色或障碍的速度？测试游戏以确保一切无缝运行。记住，好的太空飞行员总是检查设备！
