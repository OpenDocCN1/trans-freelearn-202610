

3. 输入名字和姓氏。

4. 点击 **Generate** 按钮生成新的 slug。

5. 可选地，上传头像或个人资料图片。

6. 添加标题和替代文本。

7. 输入个人简介。

8. 点击 **Publish** 按钮，如同上一节所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_51_0.png)

Figure 3.9 – New author details

我们已将示例作者替换为两位新作者。同样，还有需要处理的额外示例内容。正如本节中创建示例作者一样，也创建了多个示例文章。

## Removing and adding articles

现在我们将删除这些文章并开始创建自己的内容。这将使我们能够创建自己的网站。

### Removing example articles

按照以下步骤删除示例文章：

1. 在主 **Content** 菜单中点击 **Blog posts**。点击第一篇文章。

2. 在界面的底部，浮动栏右侧有一个上下文菜单，位于 **Publish** 按钮旁边。点击向下箭头图标，菜单将出现如下所示。 **Delete** 将是选项之一；点击它：

3. 将出现确认对话框菜单，因此点击 **Delete now** 按钮：

4. 对每篇文章重复 *步骤 1–3*。

![](img/97ab031f38e2174e2f1b51da18dfa1d0_52_0.png)

![](img/97ab031f38e2174e2f1b51da18dfa1d0_52_1.png)

现在我们已删除所有文章，我们将添加第一篇新闻文章。

### Creating a new article

按照以下步骤创建新文章：

1. 从主 **Content** 菜单中，点击 **Blog posts** 菜单项。

2. 点击铅笔图标创建新文章。

3. 为文章输入标题。

4. 点击 **Generate** 生成 slug。

5. 选择 **Published at** 日期。截至撰写本文时，此项必须选择，不能保留占位符文本：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_53_0.png)

Figure 3.12 – New blog post form, part 1

6. 通过点击 **Upload** 按钮上传图片。

7. 输入 Caption 和 Alternative text：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_54_0.png)

Caption

Alternative text
Important for SEO and accessibility.

Figure 3.13 – New blog post form, part 2

8. 通过编写文章的简短摘要来完成 Excerpt 部分。

9. 通过点击 Add 按钮选择姓名然后点击 Close 按钮来选择作者。

10. 可选地，选择一个类别。

11. 通过输入文章内容来完成 **Body** 部分：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_55_0.png)

12. 点击 **Publish** 按钮，如同之前的示例进行发布。状态应从 **DRAFT**（在 **Blog posts** 菜单中标题旁的铅笔图标表示）更改为 **Published**。

## Modifying the website settings and redeploying the website

最后，我们需要修改网站设置。设置完成后，下一步将使用本章中执行的操作重新部署网站。

### Setting the website name

返回主 **Content** 菜单，点击 **Settings** 菜单项。这些设置是网站标题、描述、关键词列表和网站作者。对于本示例，标题将为 `News and Events`：

1. 修改标题。

2. 修改描述。

3. 点击底部栏上的 **Publish** 按钮：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_56_0.png)

Figure 3.15 – Settings form

至此，我们已通过使用 Sanity Studio 创建、删除和修改内容，学习了 Sanity Studio 的基本功能。接下来，我们将指示 Netlify 部署网站的新版本。Netlify 将运行脚本以使 Gatsby 使用新内容与模板结合创建新版本。

### Redeployment through Netlify

按照以下步骤使用 Netlify 平台重新部署网站：

1. 点击 **Dashboard** 图标返回仪表板，如以下屏幕截图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_57_0.png)

Figure 3.16 – Link to the dashboard

2. 点击对应博客网站的 **Deploy** 按钮，如下所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_57_1.png)

Figure 3.17 – Netlify sites list

状态将在构建过程中从 **Success**（绿色）更改为 **Building**（橙色），如下图所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_57_2.png)

Figure 3.18 – Netlify Building status

构建时间会有所不同，但最终状态将恢复为 **Success**。

3. 点击对应 **Frontend** 的网站 URL 以查看更改。可能需要重新加载页面：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_58_0.png)

Figure 3.19 – Project info section

4. 网站将显示文章及其摘要，如图 3.20 所示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_58_1.png)

Figure 3.20 – Article excerpt

## Summary

在本章中，我们学习了如何使用 Sanity Studio 的基本功能。我们现在能够基于初始示例内容配置基本网站信息。我们可以添加、修改和删除各种内容元素。不过，我们尚无法修改 Sanity 文档的结构。

在下一章中，我们将学习 Sanity 的实体及其关系。

## 4 Sanity Configuration and Schemas

在上一章中，我们学习了如何使用 Sanity Studio。在本章中，我们将学习如何从 GitHub 克隆项目，安装其依赖项，学习如何启动本地开发环境，并检查文件结构。然后，我们将学习模式是如何构建的，以及如何修改它们，学习关系。这将使您能够轻松地修改模式，通过在浏览器中查看托管版本来在本地测试模式。

本章将涵盖的主要主题如下：

- Modeling content with schemas
- Schema relationships
- Extending, modifying, and deploying the schema

这在以下屏幕截图中显示：

![](img/97ab031f38e2174e2f1b51da18dfa1d0_61_0.png)

Figure 4.1 – The GitHub Code clone modal – SSH mode

- 第二种方法使用 HTTPS，其中 username 是 GitHub 用户名，repository 是实际仓库。请注意与 SSH 方法略有不同的格式：

```text
git clone https://github.com/username/repository.git
```

![](img/97ab031f38e2174e2f1b51da18dfa1d0_61_1.png)

Figure 4.2 – The GitHub Code clone modal – HTTPS mode

这两种方法都将在同一文件系统级别创建以仓库名称命名的文件夹。SSH 方法允许使用受密码保护的 SSH 密钥，因此是首选方法。

## Exploring the project's folders

在本节中，我们将检查项目的文件夹和文件的内容。有几个重要文件和几个文件夹。我们将检查的第一个文件是 `sanity.json` 文件。该文件位于项目的 `studio` 文件夹中。

### Examining the sanity.json file

以下是 `sanity.json` 文件的前几行：

```json
{
  "root": true,
  "project": {
    "name": "Hands On JAMstack"
  },
  "api": {
    "projectId": "abc123d4",
    "dataset": "production"
  },
}
```

该文件包含两个重要值：`projectId` 和 `dataset` 名称：

- `projectId`：`projectId` 值唯一定义项目，意味着没有其他人会拥有此 ID。`projectId` 由字母和数字组成。

- `dataset`：然而，`dataset` 值不是唯一的，将用于标识此 Sanity Studio 实例当前使用的是哪个数据集。例如，可以选择 `production` 来表示模式和数据的最终版本，而 `development` 则表示即将进行的更改。模式和实际内容都包含在数据集中。每个数据集都是完全独立的，Sanity Studio 可以加载其中任何一个数据集，但一次只能加载一个模式。

此外，`root` 设置为 `true` 以指定此文件是项目的主要 `sanity.json` 文件，因为插件可能有自己的 `sanity.json` 文件。插件中找到的 `sanity.json` 文件将 `root` 设置为 `true`。

