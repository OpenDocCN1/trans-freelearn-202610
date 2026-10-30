

## 创建您的第一个产品、图片、库存和价格

如果您点击该链接，**高级定价** 模型将显示在屏幕上，允许您为产品设置高级定价规则：

![](img/de3034a92496a32462134266292a51dd_99_0.png)

![](img/de3034a92496a32462134266292a51dd_103_0.png)

这是在产品详情页中的显示效果：

![](img/de3034a92496a32462134266292a51dd_103_1.png)

- **成本：** 这是代表产品成本的管理员信息。
- **阶梯价格：** 这是根据网站、客户组和购买数量而定的产品折扣价。
- **厂商建议零售价：** 这是产品的最低广告价格。
- **显示实际价格：** 通过此选项，您可以选择应显示 **厂商建议零售价 (MSRP)** 的位置：
  - **On Gesture：** 选择此选项时，MSRP 会在产品列表页和产品详情页中显示。此外，MSRP 价格旁边会添加一个 **Click for price** 链接，点击后会打开一个包含产品价格更多信息的弹出层。这是在产品列表页中 **On Gesture** 选项的显示效果：

![](img/de3034a92496a32462134266292a51dd_101_0.png)

- **Before Order Confirmation**：最终价格在结账的最后一步显示。MSRP 和一条消息会在产品列表、产品详情和 **Shopping Cart** 页面中显示，以通知客户结账时的价格信息。以下是 MSRP 和消息在产品列表页中的显示方式：

![](img/de3034a92496a32462134266292a51dd_104_0.png)

这是在产品详情页中的显示效果：

![](img/de3034a92496a32462134266292a51dd_105_0.png)

这里可以看到购物车页面：

![](img/de3034a92496a32462134266292a51dd_105_1.png)

## 总结

在本章中，我们学习了如何添加新产品，包括设置高级定价选项和产品图片。

在下一章中，我们将介绍属性和属性集，以向您的产品添加更多信息。

**Product** 属性是目录的核心部分。尽管您可以管理属性，但不能删除 Magento 默认包含的系统属性。为了查看系统属性列表，您应该通过 **System: Yes** 过滤网格，如以下屏幕截图所示：

![](img/de3034a92496a32462134266292a51dd_109_0.png)

如您所见，Magento 中有 43 个系统属性。您必须熟悉其中大部分，因为这些属性是我们在上一章中回顾的 **New Product** 表单字段的一部分。

## 创建产品属性

您可以在 Magento 中创建自定义产品属性，以向目录中的产品添加自定义信息。您可以通过以下步骤查看 **New Product Attribute** 表单：

1. 在 **Product** 属性网格的右上角点击 **Add New Attribute**。
2. 您将在屏幕上看到以下表单：

![](img/de3034a92496a32462134266292a51dd_110_0.png)

我们将逐一介绍表单上的每个字段：

- **Properties | Attribute Properties:**
  - **Default Label**：这是数据录入时描述新属性的标签。
  - **Catalog Input Type for Store Owner**：这为 **Product** 属性设置数据类型和输入控件。需要说明的是，一旦属性类型创建后，就无法更改。以下选项可用：
    - **Text Field**
    - **Text Area**
    - **Date**
    - **Yes/No**
    - **Multiple Select**
    - **Dropdown**
    - **Price**
    - **Media Image**
    - **Fixed Product Tax**
    - **Visual Swatch**
    - **Text Swatch**
  - **Values Required**：这定义了 **Product** 属性的值是否为必填。
- **Properties | Advanced attribute properties:**
  - **Attribute Code**：这为 **Product** 属性指定内部使用的唯一标识符。代码仅允许字母 (a-z)、数字 (0-9) 和下划线，且第一个字符应为字母。代码必须少于三十个字符且不能包含空格。
  - **Scope**：这定义属性是否可以在全局、网站或商店视图级别进行自定义。
  - **Default Value**：这是数据录入时为属性声明的默认值。
  - **Unique Value**：这设置此 **Product** 属性的值是否在作用域设置范围内唯一。例如，SKU 是一个系统属性，必须具有唯一的值来标识每个产品。
  - **Input Validation for Store Owner**：这在从管理面板创建产品时为属性值设置验证。

## 属性集和自定义属性

- **Add to Column Options：** 这将属性包含在产品网格的列选项列表中。
- **Use in Filter Options：** 这将属性包含在产品网格的过滤器选项列表中。
- **Manage Labels:**
  - **Manage Titles (Size, Color, etc.)：** 这按商店视图自定义 **Product** 属性的标签。例如，如果您有三种不同语言的商店视图，您可以为每个商店视图翻译标签。
- **Storefront Properties:**
  - **Use in Search：** 如果为 Yes，则属性将包含在搜索查询中，客户将能够根据属性值在目录中进行搜索：
    - **Search Weight：** 如果 Use in Search 设置为 Yes，则此字段将显示。您可以通过设置 1 到 10 之间的数字来设置搜索查询的优先级，其中 10 优先级最高。
    - **Visible in Advanced Search：** 如果 Use in Search 设置为 Yes，则您可以指定 **Product** 属性是否应包含在 **Advanced Search** 表单中。

![](img/de3034a92496a32462134266292a51dd_112_0.png)

注意：在 Magento 搜索中包含太多属性可能会降低搜索性能。

- **Comparable on Storefront：** 在 **Compare Products** 页面中包含 **Product** 属性。
- **Use in Layered Navigation：** 如果属性类型为 Dropdown、Multiple Select 或 Price，您可以将属性添加到产品列表页的分层导航（左侧边栏）中。
- **Use in Search Results Layered Navigation：** 如果属性类型为 Dropdown、Multiple Select 或 Price，您可以将属性添加到搜索结果页的分层导航（左侧边栏）中。

![](img/de3034a92496a32462134266292a51dd_113_0.png)

> 注意：在 Magento 分层导航中包含太多属性可能会降低产品列表页和搜索结果页的性能。

- **Position：** 这设置属性在分层导航中的位置，相对于该区块中的其他属性。
- **Use for Promo Rule Conditions：** 这将属性添加到促销规则可用条件列表中。
- **Allow HTML Tags on Storefront：** 这用于定义属性值是否可以包含用于格式化的 HTML。
- **Visible on Catalog Pages on Storefront：** 这将 **Product** 属性包含在产品详情页的 **Additional Information** 标签中。此配置仅适用于简单产品和虚拟产品。
- **Used in Product Listing：** 这将属性包含在产品列表页中。此属性配置取决于主题。
- **Used for Sorting in Product Listing：** 这将属性包含在产品列表页的 **Sort By** 选项中。此属性配置取决于主题。

现在我们回顾了 **New Product Attribute** 表单，我们准备为上一章中创建的产品创建一个自定义属性。

我们将为之前创建的 **Red Shirt** 创建一个新的产品属性——**Sleeve Length**。

在 **New Product Attribute** 表单中为该属性设置以下信息：

- **Properties | Attribute Properties:**
  - **Default Label:** `Sleeve Length`
  - **Catalog Input Type for Store Owner:** Dropdown
  - **Values Required:** No

