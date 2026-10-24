

- 10. 更改行样式选项的设置，并将“Node: Title”和“Voting API results: Function”设置为内联字段，如下所示：

Defaults: Row style options

Inline fields:
☑ Voting API results: Function
☑ Node: Title

Inline fields will be displayed next to each other rather than one after another.

Separator:

The separator may be placed between inline fields to keep them from squishing up next to each other. You can use HTML in this field.

Update Cancel

- 11. 通过点击“Sort criteria”标签旁边的 + 符号添加新的排序条件。
- 12. 新的排序条件应为“Voting API results: Function”。结果应按降序排序，如以下屏幕截图所示：

Defaults: Configure sort criterion "Voting API results: Function"

Relationship:
Voting results

Sort order:
○ Ascending
● Descending

Update Cancel Remove

- 13. 将样式修改为列表。此选项位于左侧屏幕的“Basic settings”部分。
- 14. 我们现在需要添加一个页面视图，以便 Chef Wanyama 可以查看投票结果。从视图类型的下拉列表中选择 Page，然后点击 Add display。
- 15. 将路径更改为 menu-by-rating，并将标题更改为 Menu Items by Rating。
- 16. 通过选择下拉值为 **Block** 并点击 **Add display** 按钮创建第二个显示。
- 17. 将块显示的标题更改为 **Most Popular**，将要显示的 **Items** 数量设置为 **5**，然后将 **Block Admin Settings** 设置为 **Popular menu items**。在更新标题时，请确保点击 **Override** 按钮而不是 **Update default display** 按钮，以避免更改页面的标题。
- 18. 保存视图和我们新的块，页面将可用。
- 19. 要将块添加到主页，请通过选择 **Site building** 然后从 Administer 菜单中选择 **Blocks** 打开 **Blocks Manager**。
- 20. 找到 **Popular menu items** 块并将区域更改为 **Right sidebar**。然后保存块。
- 21. 为确保块仅在主页和菜单页面（如果有）上显示，请点击 **configure** 选项，然后修改 **Page specific visibility settings**，如下图所示：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_148_0.png)

## Change the display of the ratings

Good Eatin' Goal: Customize the rating display to make the ratings more dramatic.

Additional modules needed: Fivestar (http://drupal.org/project/fivestar).

### Basic steps

The Fivestar module provides you with a wide assortment of images that can be used when displaying or selecting a rating. These are accessed by selecting Site Configuration and then Fivestar, from the Administer menu.

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_149_0.png)

The top section contains a variety of different icons that are used for display. For our Good Eatin' site, we will use the Flames setting, because that works well with our restaurant theme.

If you select one of the widgets in the Custom color widgets section, you can customize the colors used in the display to suit the theme of your site, by using the options as shown in the following screenshot:

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_149_1.png)

## Working with polls

Polls are a great way to quickly get information from your users. Visitors like to answer short polls, especially if the results are immediately available and the questions are interesting. In Drupal, polls can contain only a single question, along with a set of predefined responses that the user can choose from.

### Create a poll

Good Eatin' Goal: Create a poll to allow Chef Wanyama to get feedback from customers. The first poll will ask customers what their favorite meal is at Good Eatin'.

Additional modules needed: Poll (core).

### Basic steps

- 1. 我们开始构建投票的过程，首先确保 Poll 模块处于活动状态。Poll 模块是作为 Core Drupal 安装的一部分包含的。
- 2. 确认该模块可用后，从主导航菜单中选择 Create content 然后选择 Poll。Drupal 将打开一个新表单，您可以在其中输入有关投票的信息。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_150_0.png)

- 3. 默认情况下，您有指定两个选择的空间，但您可以通过点击 **Add another choice** 来包含额外的选择。对于我们的 Good Eatin' 网站，我们允许访客在 Breakfast、Lunch、Dinner 和 Dessert 之间进行选择。
- 4. **Poll status** 控制访客是否可以向系统中输入新的投票。如果您想手动管理投票的持续时间，您只需在投票结束时编辑投票并将 **Poll status** 设置为 Closed 即可关闭投票。
- 5. **Poll duration** 可用于在指定间隔后自动关闭投票。投票可以保持开放的时间从 1 天到 1 年不等。
- 6. 当您点击 **Save** 时，Drupal 将把投票添加到网站，并允许您对其进行投票，如以下屏幕截图所示：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_151_0.png)

- 7. 设置投票的最后一步是确保用户可以提交投票答案。这是通过 **Permissions Manager** 控制的，可通过从 **Administer** 菜单中选择 **User management** 然后选择 **Permission** 来访问。
- 8. 有多个权限可用于控制对投票的访问，如以下屏幕截图所示：

| Permission | anonymous user | authenticated user | VIP user |
|---|---:|---:|---:|
| **poll module** |  |  |  |
| cancel own vote | ☑ | ☑ | ☑ |
| create poll content | ☐ | ☐ | ☐ |
| delete any poll content | ☐ | ☐ | ☐ |
| delete own poll content | ☐ | ☐ | ☐ |
| edit any poll content | ☐ | ☐ | ☐ |
| edit own poll content | ☐ | ☐ | ☐ |
| inspect all votes | ☐ | ☐ | ☐ |
| vote on polls | ☑ | ☑ | ☑ |

- 9. 我们唯一想要授予的权限是所有用户投票和取消投票的能力。选择这些选项，然后点击 Save permissions。

### Adding the poll to the home page

Good Eatin' Goal: Add our new poll to the home page on the right-hand side, so that customers will see the poll.

Additional modules needed: Poll (core).

### Basic steps

- 1. 我们可以通过创建块将投票添加到主页。我们通过从 Administer 菜单中选择 Site building 然后选择 Blocks 来实现此操作。
- 2. 最新的投票块已经设置好以满足我们的需求，因此我们将位置更改为 Right sidebar，如以下屏幕截图所示：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_152_0.png)

- 3. 我们现在可以通过点击 configure 链接来配置显示。我们将限制投票仅在主页上显示，如以下屏幕截图所示。因为我们为主页创建了一个别名，称为 home，我们只需输入 home。但是，如果您使用的是自动生成的首页，您需要输入 <front>。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_152_1.png)

- 4. 主页上的块如下所示，在投票之前：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_153_0.png)

### Submitting a poll

*Good Eatin' Goal:* Demonstrate how a customer will submit his or her answers to our poll.

*Additional modules needed:* Poll (core).

### Basic steps

访客可以通过选择他们喜欢的选项然后点击 **Vote** 来轻松提交投票答案。

投票后，显示将更新以显示投票结果，如下所示：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_153_1.png)

### Viewing poll results

Good Eatin' Goal: Demonstrate how Chef Wanyama can view the poll results, and show the results of the poll to customers who have already answered the poll.

Additional modules needed: Poll (core).

### Basic steps

具有 Inspect all votes 权限的用户可以查看针对投票投出的所有投票的列表。这是通过查看投票并点击 Votes 选项卡来列出所有投票，如以下屏幕截图所示：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_154_0.png)

此视图对于确保所有投票都是合法的以及比较注册用户和匿名用户提交的投票非常有用。对于注册用户，将显示他们的用户名；对于未注册用户，将显示他们提交投票的机器的 IP 地址。

### Upgrading to Advanced Poll

Good Eatin' Goal: Explain some of the differences between Poll and Advanced Poll.

Additional modules needed: Advanced Poll (http://drupal.org/project/advpoll).

Advanced Poll 模块提供了核心 Poll 模块中不存在的多个附加功能，包括：除基本投票外另外三种投票系统、增强的投票管理、增强的时间管理、写入投票和改进的显示。

截至撰写本文时，Advanced poll 模块尚未移植到 Drupal 6。

