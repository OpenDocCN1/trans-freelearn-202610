

## 交易邮件和新闻通讯

### 处理新闻通讯及其订阅者

您可以在管理面板的 MARKETING | Communications | Newsletter Subscribers 页面中查看 Magento 商店的电子邮件订阅者列表。

如以下屏幕截图所示，在您首次安装 Magento 2 时，网格是空的：

![](img/de3034a92496a32462134266292a51dd_355_0.png)

如果我们前往 Storefront，我们将在页脚中找到订阅框：

如果您点击订阅，将在屏幕上显示以下确认消息：

现在，如果我们返回到 Newsletter Subscribers 页面，我们将在网格中看到新的订阅者：

![](img/de3034a92496a32462134266292a51dd_357_0.png)

从该网格中，您可以使用网格批量操作取消订阅或删除电子邮件账户；您可以导出所有订阅者，对其进行筛选，并按列对订阅者进行排序。

现在，如果您返回到 Newsletter Templates 页面（MARKETING | Communications | Newsletter Templates），您将能够通过使用最后一列中的下拉菜单将新闻通讯模板添加到队列中：

![](img/de3034a92496a32462134266292a51dd_358_0.png)

您将被重定向到 **Edit Queue** 页面，您可以在此设置 **Queue Date Start** 以指定何时发送新闻通讯电子邮件并编辑新闻通讯详细信息：

![](img/de3034a92496a32462134266292a51dd_358_1.png)

保存模板以将其包含在队列中后，您会发现该模板处于待处理状态，等待发送到 **Newsletter Queue** 页面（**MARKETING | Communications | Newsletter Queue**）：

![](img/de3034a92496a32462134266292a51dd_359_0.png)

如果您点击该行，您将被重定向到之前的同一页面，您可以在此设置 **Queue Date Start** 以指定何时发送新闻通讯并修改新闻通讯信息。

### 总结

在本章中，我们介绍了修改交易邮件和新闻通讯的过程，并安排了一个新闻通讯模板发送给新闻通讯订阅者。在下一章中，我们将创建报告以跟踪商店销售、客户活动以及与商店目录相关的统计数据。

## 19 报告

本章我们将介绍以下概念：

- Marketing reports
- Sales reports
- Customer reports
- Product reports

### Magento 报告

作为商店所有者和管理员，您的一项关键活动是跟踪 Magento 商店的活动和转化率。Magento 包含多个报告，您可以定期查看，以便始终能够查看商店的统计数据。

所有报告都可以通过 Magento 管理面板中的同一菜单项访问：Reports

![](img/de3034a92496a32462134266292a51dd_361_0.png)

如前图所示，Magento 包含 22 个报告，分为 5 个组，如下所示：

- **Marketing:**
    - Products in Carts
    - Search Terms
    - Abandoned Carts
    - Newsletter Problem Reports
- **Reviews:**
    - By Customers
    - By Products
- **Sales:**
    - Orders
    - Tax
    - Invoiced
    - Shipping
    - Refunds
    - Coupons
    - PayPal Settlement
    - Braintree Settlement
- **Customers:**
    - Order Total
    - Order Count
    - New
- **Products:**
    - Views
    - Bestsellers
    - Low Stock
    - Ordered
    - Downloads

除此之外，您将在列表末尾看到一个额外的菜单项：**Refresh Statistics.**

为了优化性能，Magento 会计算并存储每个报告的信息，而不是即时计算所有内容。为了在报告中看到最新信息，您应该从 Magento 管理面板的该页面刷新统计数据。

如以下图所示，有两个批量操作可供选择，以通过刷新生命周期统计数据或仅最近一天的数据来更新所选报告：

![](img/de3034a92496a32462134266292a51dd_364_0.png)

如果选择 **Refresh Lifetime Statistics**，确认消息会说明更新可能会降低商店的性能：

![](img/de3034a92496a32462134266292a51dd_365_0.png)

如果您确认更新，您的报告数据将是最新的，并且网格中的 **Updated** 日期将显示报告更新的时间：

![](img/de3034a92496a32462134266292a51dd_366_0.png)

让我们回顾一下每个报告：

- **Marketing | Products in Carts:** 营销组中的第一个报告是 **Products in Carts** 报告。正如名称所示，您可以看到当前在商店前端的客户购物车中或管理面板中待处理订单中的产品。
- **Marketing | Search Terms:** 营销组中的第二个报告是 **Search Terms** 报告，您可以借此查看 Magento 商店中最受欢迎的搜索查询。
- **Marketing | Abandoned Carts:** 下一个报告允许您查看客户的弃购购物车，这对于再次联系这些客户以激励他们完成订单非常重要。
- **Marketing | Newsletter Problem Reports:** 如果新闻通讯投递出现错误，您将能够从此报告中查看和审查这些错误，所有错误详细信息都位于 `var/log` 文件夹中的日志文件中。
- **Reviews | By Customers / By Products:** 通过这些报告，您将能够更仔细地查看商店中评论的统计数据，按客户或按产品对这些评论的统计数据进行分组。
- **Sales | Orders / Tax / Invoiced / Shipping / Refunds / Coupons:** 查看与商店中订单、税率、发票、运输、贷项通知单和优惠券相关的所有统计数据。对于所有报告，都有一个筛选器可按时间段、日期范围和订单状态查看报告。
- **Sales | PayPal Settlements / Braintree Settlements:** 查看影响 PayPal 和 Braintree 资金结算的每笔交易的信息。
- **Customers | Orders Total:** 查看在给定时间段内每个客户订单的总金额，包括每个客户的订单平均金额。
- **Customers | Order Count:** 查看在给定时间段内每个客户的订单总数，包括每个客户的平均订单数。
- **Customers | New:** 特定时间段内间隔的新客户数量。
- **Products | Views:** 查看在特定时间段内商店中每个产品的浏览次数。
- **Products | Bestsellers:** 查看给定时间段的畅销书列表。
- **Products | Low Stock:** 查看当前库存量低的产品列表。
- **Products | Ordered:** 查看所选时间段内已订购产品的列表和售出数量。
- **Products | Downloads:** 查看与可下载产品相关的统计数据，包括购买和下载。

### 总结

在本章中，我们回顾了 Magento 2 中现有的报告，并回顾了如何刷新统计数据以从每个报告中获取最新信息。在下一章中，您将了解如何管理 Magento 主题、内联翻译、Magento Marketplace 等，以便能够自定义您的 Magento 商店。

## 20 自定义您的 Magento 商店

本章我们将介绍以下主题：

- How to manage Magento themes
- How Inline Translation works
- Magento marketplace

### 如何管理 Magento 主题

Magento 中的主题定义了客户 Storefront 的外观和感觉。默认情况下，Magento 包含以下主题：

- Magento Blank
- Magento Luma

