

# 示例中的 `logic_hooks.php` 文件如下所示：

```php
<?php
$hook_version = 1;
$hook_array = Array();
$hook_array['before_save'][] = Array(1, 'NameCasing',
                                     'custom/modules/Contacts/ProperCasing.php',
                                     'PC', 'convertToProper');
?>
```

除最后一行外，前面的所有代码均为标准代码。最后一行至关重要，因为它告诉 SugarCRM 在何时执行代码（`before_save`）、哪个文件包含相关代码（`ProperCasing.php`）、包含相关代码的类名，以及最后，类中应执行代码的函数。

如果你对前两个参数（1 和 NameCasing）有疑问，它们是任意值，你可以随意定义。该数字用于对逻辑钩子进行排序，适用于你有多个逻辑钩子的情况，而第二个值仅仅是当前逻辑钩子的标签。

> 要添加同类型的其他逻辑钩子，只需复制此最后一行，并更新其中包含的各个参数即可。

接下来，我们必须创建上面引用的文件，即 `ProperCasing.php`，该文件将包含确保数据以正确大小写输入的 PHP 代码。该文件如下所示：

```php
<?php
class PC {
    function convertToProper(&$bean, $event, $arguments)
    {
        $bean->first_name = ucwords($bean->first_name);
        $bean->last_name = ucwords($bean->last_name);
    }
}
?>
```

你会发现实际上只需要很少的代码。`$bean` 引用是访问当前记录和数据库连接的方法。这使我们能够检查受影响模块中当前记录所包含的任何字段中的数据，并允许我们更改其值。

最后一步是将文件放置在正确的位置，以指示 SugarCRM 在用户使用联系人模块时调用代码。为此，`logic_hooks.php` 和 `ProperCasing.php` 文件都需要放置在安装目录的 `custom/modules/Contacts` 文件夹中。如果目录结构不存在，请通过 Windows 资源管理器、FTP 客户端等手动创建。确保模块名称的大小写和拼写与 `sugar install directory/modules/` 文件夹中找到的名称一致。

文件放置在正确的文件夹后，尝试输入一个名和姓字段均为全小写值的联系人。观察记录保存后这两个字段会发生什么变化。

可以通过各种 PHP 文件对系统应用许多其他自定义。这包括更改默认字段和自定义字段的行为、修改各种屏幕等。最棒的是，这些更改也是升级安全的，执行此类更改的能力是 SugarCRM 架构的核心，我们将现在对其进行考察。

## 模型-视图-控制器（MVC）

SugarCRM 的架构采用 MVC 设计模型，从而使其成为高度可定制的应用程序。该模型的目标是将系统的各个组件分离到功能区域，包括数据（模型）、用户界面方面（视图）和数据库操作（控制器）。它首次在 SugarCRM 的 5.0 版本中引入，并使系统的可定制性显著提高。

你可以进行的一些潜在更改包括以下内容：

- 具有特殊行为的自定义列表、详细信息和编辑屏幕
- 在各种视图上添加或删除按钮
- 创建依赖下拉列表
- 插入自定义 JavaScript
- 抑制特定子面板
- 数据的条件样式

可能性几乎是无限的，仅受限于你的想象力和编程能力。让我们看几个快速示例，让你体验这种架构的便捷和强大。

在第一个示例中，我们将更改案例模块中名称字段的默认行为，从文本更改为下拉列表。这将允许我们将分配给该字段的值限制为列表中定义的值。这是一种有助于强制执行一致性并改进指标的技术。

请注意，创建下拉列表字段需要指定一个包含用户可选择值的下拉列表。我们将假设本练习中使用的下拉列表名为 `cases_names`，该列表之前是使用下拉列表编辑器工具创建的。

要修改名称字段的默认行为并将其转换为下拉列表，请创建一个包含以下内容的 PHP 文件：

```php
<?php
$dictionary['Case']['fields']['name']['type']='enum';
$dictionary['Case']['fields']['name']['options']='cases_names';
?>
```

保存文件并将其命名为 `vardefs.ext.php`。请注意，文件虽然很小，但非常强大。第一行覆盖了名称字段定义的默认类型，并将其更改为 enum，这是 SugarCRM 用于下拉列表字段的值。第二行指定字段应使用的下拉列表的名称。如前所述，我们想要使用的值所在的下拉列表名为 cases_names。

接下来，你需要将文件放置在名为 `sugar install directory/custom/Extension/modules/Cases/Ext/Vardefs` 的目录中。如果目录结构不存在，请手动创建目录结构，并确保与前面所示的大小写匹配。

最后，需要将更改应用到 SugarCRM 系统。这是通过管理控制面板中的 **Repair** 选项完成的。以管理员级用户身份登录 SugarCRM，然后选择 **Admin | Repair | Quick Rebuild & Repair | Repair** 以应用修改。_如果不执行此步骤，你的修改将不会显示！_

修复过程完成后，检查你的案例模块，它现在应该将名称字段显示为下拉列表字段。

让我们进一步扩展此示例，假设我们还希望在访问案例的详细信息视图时，通过将 **Assigned To** 字段的文本颜色设为红色来突出显示它。这样做有助于引导用户的视线关注关键信息，并可用于你认为同样重要的任何其他字段。

## 模块构建器

你可能会发现，在审查 SugarCRM 系统时，它可能不包含适合存储对业务运营至关重要的信息的模块。例如，RayDoc 可能希望存储有关各种工作所用材料的信息，并将其链接到相应的客户条目。这样，RayDoc 的任何人都可以查看执行特定工作所用材料的列表。

显然，利用 MVC 模型可以实现许多事情，多到无法在本书中全部介绍。幸运的是，SugarCRM Wiki 和 Developer Blog 包含有关此主题的更详细信息（以及更多示例）。你可以在以下位置访问它们：

- http://www.sugarcrm.com/wiki/index.php?title=Model-View-Controller_(MVC)
- http://developer.sugarcrm.com

要完成示例并应用更改，请确保再次在 **Admin** 控制面板中使用 **Repair** 选项。

customCode 部分是实现我们目标的关键。这种技术允许你将自定义 HTML/JavaScript/CSS 注入到 SugarCRM 的默认视图配置中。请注意，虽然我们特别使用 DetailView，但类似的技术也可以用于其他视图。

```php
array('name' => 'case_number', 'label' => 'LBL_CASE_NUMBER'),
array('name' => 'assigned_user_name', 'customCode' => '
<span style="color: red">${fields.assigned_user_name.value}</span>'),
```

前面的部分对应于包含 Assigned To 值的显示行的定义。要修改它以红色显示此值，请将前面的代码更改为反映以下内容：

```php
array (
array('name' => 'case_number', 'label' => 'LBL_CASE_NUMBER'),
'assigned_user_name',
),
```

使用你选择的文本编辑器编辑 detailviewdefs.php 并找到以下内容：

实现此目标的最简单方法是复制默认的 DetailView 定义文件（detailviewdefs.php），该文件存储在 sugar install directory/modules/Cases/metadata，并将其放置在 sugar install directory/custom/modules/Cases/metadata。同样，如果目录结构不存在，请手动创建。

## 自定义 SugarCRM

