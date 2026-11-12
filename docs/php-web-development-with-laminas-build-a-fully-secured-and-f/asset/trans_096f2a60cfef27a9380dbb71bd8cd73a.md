

`class_name` 字段是 `classes` 表中字段名的别名。我们已在通过 `Select` 类创建的 `INNER JOIN` 中定义了该别名。

虽然我们在内存中将数据作为对象处理，但需要将数据转换为数组以将其持久化到表中。因此，在 `Student->toArray` 方法中，我们将对象替换为主键值：

```
public function toArray()
{
    $attributes = get_object_vars($this);
    $attributes['class_code'] = $attributes['schoolClass']
        ->code;
    unset($attributes['schoolClass']);
    return $attributes;
}
```

要插入新学生，我们需要将其与学校班级关联。这需要在学生的编辑表单中提供学校班级列表。在下面的代码部分，我们可以看到 `view/student` 的 `edit.php` 文件中创建此列表的部分：

```
<select name="class_code">
<?php
$schoolClasses = SchoolClassTable::getInstance()->getAll();
foreach($schoolClasses as $schoolClass) :
?>
<option value="<?=$schoolClass->code?>"
    <?=($schoolClass->code == $Student->schoolClass->code ?
    'selected="selected"' : '')?>>
    <?=$schoolClass->name?></option>
<?php
endforeach;
?>
</select>
```

这些是学校班级与学生注册之间的主要区别。`school2` 项目完整可用于 `https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter04/school2`。针对学生注册有一个 `StudentTest` 测试用例和一个代码覆盖率报告。尝试完整的应用程序并比较两个注册的代码。在 Eclipse 中，您可以通过选择两个文件并单击鼠标右键，然后选择 **与其他项比较/互相比较** 来比较两个文件。

现在您知道了如何抽象数据库连接以及如何使用 `laminas-db` 创建 ORM。

在下一节中，我们将重新实现学校班级和学生注册，但这次我们将使用 `laminas-mvc` 组件作为应用程序的容器。

## 将 Laminas 用作容器

当您将 Laminas 用作库时，您的代码会调用 Laminas 组件。这就是我们在 `school1` 和 `school2` 项目中所做的。现在，我们将使用 Laminas 作为框架，该框架将调用您的代码。以前，Laminas 包含在我们的应用程序中。现在，我们将创建一个将包含在框架中的应用程序。实际上，我们将根据 Laminas 的 MVC 结构重写我们的代码。

由于这是为更复杂和完整的应用程序做准备，我们将仅展示学校班级列表的实现，但请不要担心，因为完整代码可在本节末尾获取。但请尝试在从存储库复制代码之前，先根据说明自行实现代码，以便您可以将自己的预期与提供的代码进行比较。

与之前一样，我们需要通过创建新项目来为实现做准备。让我们来创建吧！

## 创建 school3 项目

要创建 `school3` 项目，请执行以下步骤：

- 1. 创建项目。

  您还记得我们在 Eclipse 工具栏中配置了一个创建 Laminas 项目的选项吗？我们在上一章中完成了此操作。好吧，您可以使用它来创建 `school3` 项目。

  使用 `File | New | PHP Project` 菜单选项打开项目。

- 2. 将 PHPUnit 作为开发依赖项添加到 `school3` 项目。

- 3. 不要忘记保存 `composer.json` 文件并单击 **更新依赖项** 按钮。

> **允许插件**
> 
> 在从骨架创建项目后安装组件时，您可能会被问及 **您是否信任 ‘laminas/laminas-component-installer’ 执行代码并希望立即启用它？** 以及 **您是否信任 ‘laminas/laminas-skeleton-installer’ 执行代码并希望立即启用它？**。这些消息与 Composer 插件有关。您可以回答 y。

- 4. 安装 PHPUnit 后，右键选择 `school3` 项目并访问 Run As | PHPUnit Test 项。您将在 Console 选项卡上看到以下消息：

```
Fatal error: Uncaught Error: Class "Laminas\\Test\\PHPUnit\\Controller\\AbstractHttpControllerTestCase" not found
in /opt/lampp/htdocs/school3/module/Application/test/
Controller/IndexControllerTest.php:11
```

此错误发生是因为 Laminas 骨架项目依赖于 `laminas-test` 组件，这是一个用于 PHPUnit 的扩展，允许我们测试 Laminas 控制器。由于您已在 `composer.json` 编辑器中安装了 PHPUnit，请搜索 `laminas/laminas-test` 包并安装它。我们在 *第 3 章，使用 Laminas 作为库进行测试驱动开发* 中学习了如何安装 Composer 包。

- 5. 现在您可以成功运行测试了。但运行的是哪个测试呢？是 `module/Application/test/Controller` 文件夹中的 `IndexControllerTest` 类。

> **在 PHP 7 中测试 Laminas MVC 时出错**
> 
> 您可能记得，在 *第 2 章，为我们的电子商务应用程序设置环境* 中，我们展示了 `laminas-mvc` 的 PHP 要求。`composer.json` 文件允许 PHP 7。但是，如果在 PHP 7 下运行 `IndexControllerTest`，则 `testIndexActionViewModelTemplateRenderedWithinLayout` 测试方法会出现错误。事实上，错误是因为 `$this->assertQuery` 方法依赖于仅从 PHP 8 开始可用的联合类型。您可以将 `$this->assertQuery` 替换为其他断言方法，并在 PHP 7 下继续使用 `laminas-test`，但最好将 PHP 升级到 8 版本。但是，如果出于某些原因必须使用 PHP 7，则删除 `testIndexActionViewModelTemplateRenderedWithinLayout` 方法。

既然我们已运行测试，现在将创建一个新模块。

## 创建 School 模块

Laminas MVC 应用程序被组织为一组模块。Laminas 中的模块是最大的复用单元。模块是自包含的。它有自己的配置和自己的 MVC 实现。要使用 `laminas-mvc` 实现我们的学校班级注册，我们将创建另一个名为 `School` 的模块。Laminas 模块位于 `modules` 文件夹中。有一个名为 `Application` 的默认模块。`Application` 模块包含您在上一章中查看的示例主页。

我们将使用 `Application` 模块作为新模块的模型：

- 1. 将 `modules` 文件夹中的 `Application` 文件夹复制到同一 `modules` 文件夹中的 `School`。

Laminas 中模块的命名空间就是模块的名称。因此，Application 模块的命名空间是 Application，那么 School 模块的命名空间必须是 School。在重建实现时，我们将更改每个文件的命名空间。让我们从模块的最小代码开始。

- 2. 在 modules.config.php 文件中注册模块。

modules.config.php 文件位于项目根目录的 config 文件夹中。此文件包含以 PHP 数组形式列出的启用模块列表。列表之外的模块会被忽略，就好像它们不存在一样。这使得通过从 modules.config.php 中删除模块来禁用模块变得容易。您可以看到 Application 是最后一项。这不是随机的。最后一个模块是决定布局的模块。布局是视觉元素的排列。对于 Web 应用程序，布局是使用层叠样式表 (CSS) 定义的。我们的新模块 School 没有布局，因此不能是最后一个。

- 3. 将 School 作为 Application 之前的一项添加。

modules.config.php 中列出的模块会被查找。但是，只有当模块具有自动加载配置时，才会找到它们。您应该记得我们已为 school1 项目通过 Eclipse 配置了自动加载。您可以使用相同的功能来配置 school3 的 composer.json。

- 4. 在 Composer 代码编辑器中选择 Autoload 选项卡。您将看到 Application 模块已有一个 PSR-4 配置。您必须添加一个带有以下参数的新自动加载配置：

  - * **Namespace:** `School\\`
  - * **Paths:** `module/School/src/`

我们还必须为测试类添加一个 PSR-4 自动加载配置。

- 5. 选择 Autoload-dev 选项卡。添加一个带有以下参数的配置：

  - * **Namespace:** `SchoolTest\\`
  - * **Paths:** `module/School/test/`

- 6. 保存 composer.json 文件并单击 **更新依赖项（包括 require-dev）** 按钮。

> **直接编辑 composer.json**
> 
> 我们使用特定表单所做的每项更改也可以直接进行。您可以单击 composer.json 视图底部的 **composer.json** 选项卡。编辑器将文件作为文本文件打开。您可以看到其他选项卡所做的更改，也可以直接进行更改。

