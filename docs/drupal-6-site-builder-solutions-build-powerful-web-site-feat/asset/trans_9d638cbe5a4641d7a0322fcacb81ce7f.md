

## 添加产品和服务

- 2. 接下来，点击 **Add** 字段链接。Drupal 将显示一个表单，您可以在其中定义新字段的基础信息。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_93_0.png)

- 3. 对于价格字段，将 **Field name** 设置为 Price，将 **Label** 设置为 Price，并将 **Field type** 设置为 Decimal。然后点击 **Continue**。

- 4. 点击 **Continue** 按钮后，Drupal 会提示您选择用于编辑字段的 **Widget type**。对于价格字段，**Text field** 是最合适的 Widget。

- 5. 再次点击 **Continue**，Drupal 将显示一个包含一些标准设置以及根据您选择的 **Field type** 而变化的设置的新表单。

- 6. 对于价格字段，我们将更新 **Help text** 和 **Default Price**，如以下屏幕截图所示：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_93_1.png)

7. 还有一些特定于 Decimal 字段类型的数值。这些设置允许您自定义允许的值和各种格式选项。我们将为价格字段使用以下值：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_94_0.png)

## 为菜单项添加季节信息

*Good Eatin' Goal:* 创建存储菜单项是否季节性的信息字段，如果是季节性的，则存储菜单项可供使用的日期。

*Additional modules needed:* CCK (http://drupal.org/project/cck), Date (http://drupal.org/project/date).

Chef Wanyama 喜欢根据季节轮换菜单，以确保他的菜肴中始终包含最新鲜的食材。为了支持这一点，我们将允许他指定每个项目是否季节性，如果是季节性，则指定菜单应可供使用的日期。菜单的显示将考虑这些日期，仅显示当前可用的菜单项。

### 基本步骤

- 1. 我们将通过添加 Seasonal 字段开始，该字段是使用 Select list widget 的文本字段。此字段的添加方式与 Price 字段类似，只是我们将使用 Select list 作为 Widget type。我们将通过设置 Allowed values list 来确保有效值仅为 Yes 和 No，如以下屏幕截图所示：
- 2. 要添加开始和结束日期，您需要添加第二个字段，使用 Date 字段。
- 3. 为了访问 Date 字段，您必须先从 Drupal 网站下载 Date Module 并将其安装到您的网站上。最低限度，您需要确保激活 Date API、Date、Date Timezone、Date Popup 和 Calendar 模块。

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_95_0.png)

| Enabled | Name | Version | Description |
|---|---|---|---|
| ☑ | **Date** | 6.x-2.0-rc4 | 定义 CCK 日期/时间字段和 widget。依赖于：Content（已禁用）、Date API（已禁用）、Date Timezone（已禁用）。**Required by:** Date Copy（已禁用） |
| ☑ | **Date API** | 6.x-2.0-rc4 | 可供其他模块使用的 Date API。Required by: Date（已禁用）、Date PHP4（已禁用）、Date Popup（已禁用）、Date Repeat API（已禁用）、Date Timezone（已禁用）、Date Copy（已禁用） |
| ☐ | **Date Copy** | 6.x-2.0-rc4 | 导入和导出 CCK 日期数据。依赖于：Content（已禁用）、Date（已禁用）、Date API（已禁用）、Date Timezone（已禁用） |
| ☐ | **Date PHP4** | 6.x-2.0-rc4 | 在 PHP 4.x、PHP 5.0 和 PHP 5.1 中模拟 PHP 5.2 日期函数。使用 Date API 且 PHP 版本低于 PHP 5.2 时需要。依赖于 Date API（已禁用） |
| ☑ | **Date Popup** | 6.x-2.0-rc4 | 启用 jQuery 弹出日历和时间输入 widget 用于选择日期和时间。依赖于：Date API（已禁用）、Date Timezone（已禁用） |
| ☐ | **Date Repeat API** | 6.x-2.0-rc4 | 用于根据 iCal 规则计算重复日期和时间的 Date Repeat API。依赖于 Date API（已禁用） |
| ☑ | **Date Timezone** | 6.x-2.0-rc4 | 使用 Date API 时需要。覆盖站点和用户时区处理以设置时区名称而不是偏移量。依赖于 Date API（已禁用）。Required by: Date（已禁用）、Date Popup（已禁用）、Date Copy（已禁用） |

- 4. 现在适当的 Date 模块已启用，我们可以添加新字段。开始和结束日期被实现为使用 *jQuery pop-up calendar* 的单个 **Date** 字段，如果输入了开始日期，则结束日期为必填。
- 5. 开始添加 Price 字段和季节字段的方式，通过点击 **Add field** 链接。这次，我们将字段命名为 **Seasonal Dates** 并将 **Widget type** 设置为 **Text Field** with *Date Pop-up calendar*。点击 **continue** 以添加字段并输入高级信息。

## 添加产品和服务

6. 我们现在可以输入日期字段的基础信息。您需要选择的基本选项如下：

![](img/b332d58f89551dfc3ae9a7cf47e6a08f_97_0.png)

7. 我们现在可以输入季节日期的高级信息。由于本例中小时和分钟不重要，我们将从显示中移除它们。我们还将确保字段不是必填的，因为并非所有菜肴都是季节性的。高级选项设置如下：

## 全局设置

这些设置适用于 *Seasonal dates* 字段在出现的每个内容类型中的设置。

- - ☐ Required

**Number of values:**

`1`

为此字段选择特定数量的值，或选择“Unlimited”以提供“Add more”按钮，以便用户添加任意数量的值。

**Warning:** 在创建数据后更改此设置可能会导致数据丢失！

**To Date:**

○ Never

○ Optional

◉ Required

显示匹配的第二个日期字段作为“To date”。如果标记为“Optional”，字段将显示但不是必填的。如果标记为“Required”，则在“From date”为必填或已填写时，“To date”将为必填。

**Granularity:**

- ☑ Year
- ☑ Month
- ☑ Day
- ☐ Hour
- ☐ Minute
- ☐ Second

设置要存储的日期元素（至少需要一年）。

### 默认显示

**Date display:**

`June 19, 2008`

**Custom display format:**

` `

### 其他显示设置

**Time zone handling:**

`No time zone conversion`

选择用于此日期字段的时区处理方法。

## 添加菜单项可供使用的餐次

Good Eatin' Goal: 添加字段以确定项目可供使用的餐次。这将用于为每餐构建菜单。

Additional modules needed: CCK (http://drupal.org/project/cck).

Meals 字段与 Seasonal 字段类似，因为它是一个具有选择列表 widget 的文本字段。然而，我们将允许编辑者从我们的列表中选择多个选项。

### 基本步骤

- 1. 编辑 Menu Item 内容类型。
- 2. 添加一个名为 meals 的新文本字段，将标签设置为 Available at these meals，widget 为 Select List。
- 3. 将 Allowed values list 设置为：
  - Breakfast
  - Lunch
  - Dinner
  - Dessert
- 4. 选择 Required 复选框并将 Number of values 设置为 Unlimited。
- 5. 保存新字段。

## 为菜单项添加图片

Good Eatin' Goal: 允许 Chef Wanyama 可选地为菜单中显示的项目添加图片。

Additional modules needed: CCK (http://drupal.org/project/cck), IMCE (http://drupal.org/project/imce), IMCE CCK Image (http://drupal.org/project/imceimage).

### 基本步骤

- 1. 首先下载并安装 IMCE 和 IMCE CCK Image 模块。
- 2. 创建一个新字段，点击 Add field 链接。命名该字段为 Picture 并将标签设置为 Picture。类型将为 IMCE image 并使用 IMCE image widget。

3. 点击 continue 创建字段后，您可以输入字段的设置。这些设置允许您控制可以插入的图片类型，以及是否必须输入图片。

# Picture

[more help...]

Menu Item settings

这些设置仅适用于 Menu Item 内容类型中出现的 Picture 字段。

Valid File Types:*

png,gif,jpg,jpeg

Help text:

Enter a picture of the item for sale.

Instructions to present to the user below this field on the editing form.
Allowed HTML tags: <a> <b> <big> <code> <del> <em> <i> <ins> <pre> <q> <small> <span> <strong> <sub> <sup> <tt> <ol> <ul> <li> <p> <br> <img>

Default value

Global settings

这些设置适用于 Picture 字段在出现的每个内容类型中的设置。

Required

Number of values:

1

用户可以为此字段输入的最大值数量。
“Unlimited”将提供“Add more”按钮，以便用户添加任意数量的值。
Warning! 在创建数据后更改此设置可能会导致数据丢失！

Save field settings

4. 如果您希望确保每个项目都有图片，您可能希望展开 Default value 部分并指定要使用的默认图片。默认图片通常是带有图形的通用图片，显示图片未找到或不可用。

5. 我们现在可以通过点击 Save field settings 来保存字段设置。

