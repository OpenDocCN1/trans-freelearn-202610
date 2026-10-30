

![](img/de3034a92496a32462134266292a51dd_339_0.png)

# 发票、发货和贷项通知单

生成和管理发票、发货以及贷项通知单的过程非常简单，且对它们而言流程相同。

如果进入订单页面，你会看到之前介绍过的标签页，包括以下内容：

- Invoices
- Credit Memos
- Shipments

从这两个部分中，如我们之前所见，可以看到该订单的发票和贷项通知单列表。

## 履行订单

你可以通过点击屏幕右上角编辑按钮旁边的 Invoices 按钮为订单创建发票：

![](img/de3034a92496a32462134266292a51dd_340_0.png)

点击该按钮后，你将看到发票中将包含的所有信息的预览，并且可以添加注释并通过电子邮件发送发票副本：

![](img/de3034a92496a32462134266292a51dd_340_1.png)

点击 **Submit Invoice** 后，发票将被生成且订单状态会被更新。接下来，你可以为订单创建发货。

如果进入 **Shipments** 标签页，你将看到为当前订单创建的发货列表：

![](img/de3034a92496a32462134266292a51dd_341_0.png)

要创建发货，应点击屏幕右上角的 Ship。你将被重定向到以下页面：

![](img/de3034a92496a32462134266292a51dd_342_0.png)

在该页面上，你可以编辑收货和账单地址，为已发货产品添加追踪号码，指定将要发货的商品，添加注释，并通过电子邮件发送发货副本。一旦点击 Submit Shipment，发货将被创建，订单状态会被更新，且发货信息可从发货标签页中查看：

![](img/de3034a92496a32462134266292a51dd_343_0.png)

当订单已开具发票后，你可以通过点击屏幕右上角的 **Credit Memo** 按钮创建贷项通知单。你将被重定向到以下页面：

![](img/de3034a92496a32462134266292a51dd_343_1.png)

在那里，你可以标记要退回库存的商品，设置退款数量，为运费退款指定特定金额，并调整退款和费用。此外，如我们对发票和发货所见，你可以添加注释并通过电子邮件发送副本：

![](img/de3034a92496a32462134266292a51dd_344_0.png)

如果点击 **Refund Offline**，贷项通知单将被生成，并在屏幕上显示以下消息：

![](img/de3034a92496a32462134266292a51dd_345_0.png)

# Summary

在本章中，我们回顾了 Magento 中订单的生命周期。在下一章中，我们将介绍如何在 Magento 中修改交易性电子邮件并管理商店通讯。

# 18

# 交易性电子邮件和通讯

在本章中，我们将涵盖以下主题：

- Modifying transactional e-mail templates
- Modifying newsletter templates
- Handling newsletters and their subscribers

## Modifying transactional e-mail templates

Magento 提供了启动你的电子商务商店所需的所有交易性电子邮件模板。交易性电子邮件是 Magento 在商店中发生特定操作时自动发送给客户的电子邮件，例如用户创建客户账户或下达新订单时。

你可以通过管理面板中的 **MARKETING | Communications | Email Templates** 页面修改 Magento 2 中包含的默认交易性电子邮件。

如果首次打开该页面，你会看到网格为空：

![](img/de3034a92496a32462134266292a51dd_347_0.png)

只有由管理员修改过的交易性电子邮件才会显示在 Email Templates 网格上。你可以通过点击 Add New Template 按钮修改或添加交易性电子邮件模板。

**New Template** 页面将会显示：

![](img/de3034a92496a32462134266292a51dd_348_0.png)

如 **Template** 下拉列表中所示，这是 Magento 2 中包含的交易性电子邮件列表：

- **Customer Account:**
  - New Account
  - New Account Confirmation Key
  - New Account Confirmed
  - New Account Without Password
  - Forgot Password
  - Remind Password
  - Reset Password
  - Change Email
  - Change Email and Password
- **Customer Activity:**
  - Contact Form
  - Send Product Link to Friend
  - Wishlist Sharing
- **Newsletters:**
  - Subscription Confirmation
  - Subscription Success
  - Unsubscription Success
- **Product Alert:**
  - Cron Error Warning
  - Price Alert
  - Stock Alert
- **Promotions:**
  - Promotion Notification / Reminder
  - Admin Activity
  - Forgot Admin Password
  - Reset Password
- **Email Templates:**
  - Email – Header
  - Export Failed
  - Import Failed
- **Product Alert:**
  - Product Alerts Cron Error
  - Product Price Alert
  - Product Stock Alert
- **Order:**
  - New Order
  - New Order for Guest
  - Order Update
  - Order Update for Guest
  - Payment Failed
- **Invoice:**
  - New Invoice
  - New Invoice for Guest
  - Invoice Update
  - Invoice Update for Guest
- **Shipment:**
  - New Shipment
  - New Shipment for Guest
  - Shipment Update
  - Shipment Update for Guest
- **Credit Memo:**
  - Credit Memo Update
  - Credit Memo Update for Guest
  - New Credit Memo
  - New Credit Memo for Guest
- **System Notifications:**
  - Sitemap Generate Warnings
  - Currency Update Warnings

如果选择模板并点击 **Load Template** 按钮，模板内容将被加载到表单中，如以下截图所示：

![](img/de3034a92496a32462134266292a51dd_350_0.png)

在该表单中，你可以为模板设置描述性名称，设置主题和内容，并添加自定义样式以渲染模板布局。

此外，你可以通过点击屏幕顶部的 **Convert to Plain Text** 按钮移除模板中的所有 HTML 标签。

在该按钮旁边，你会找到 **Preview Template**，它允许你确切地看到模板将如何显示给客户。

如以下截图所示，模板已加载到 Magento 管理面板中：

![](img/de3034a92496a32462134266292a51dd_351_0.png)

电子邮件的标志在管理面板的以下部分配置：

**CONTENT | Design | Configuration**

我们将在 `Chapter 20`，*Customizing your Magento Store* 中介绍该部分。

在添加详细信息并预览电子邮件模板后，你可以通过点击 **Save Template** 按钮保存新模板。

你将被重定向到 **Email Templates** 网格，并会看到确认消息：

![](img/de3034a92496a32462134266292a51dd_352_0.png)

## Modifying newsletter templates

你可以在管理面板的 **MARKETING | Communications | Newsletter Templates** 页面中查看现有通讯模板列表。

如以下截图所示，在默认的 Magento 2 安装中，网格为空：

![](img/de3034a92496a32462134266292a51dd_353_0.png)

你可以通过点击屏幕右上角的 **Add New Template** 按钮添加通讯模板。该表单与新的交易性电子邮件模板表单非常相似，并在顶部包含相同的操作：

![](img/de3034a92496a32462134266292a51dd_354_0.png)

由于表单非常相似，且操作与我们之前回顾的表单相同，我们将继续保存新模板。

新模板已保存，并在屏幕上显示确认消息：

![](img/de3034a92496a32462134266292a51dd_354_1.png)

