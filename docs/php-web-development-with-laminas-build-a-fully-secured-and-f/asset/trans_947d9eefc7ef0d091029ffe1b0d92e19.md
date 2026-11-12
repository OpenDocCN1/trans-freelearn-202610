

>> <snippet_undefined><snippet_keyword>default</snippet_keyword> 套件有未定义的步骤。请选择上下文以生成代码片段：</snippet_undefined>

[0] None
[1] FeatureContext
>

前面的输出表明存在一个未定义的场景。为什么？因为没有包含实现该场景代码的上下文类。最终，Behat 会等待你为该场景选择生成代码片段的目标。你可以选择 None，这样什么都不会发生，或者选择默认的上下文类 FeatureContext。现在，请选择 None，因为我们将为我们的场景创建一个特定的类。

现在，将 features/bootstrap 文件夹中的 FeatureContext.php 文件复制到同一文件夹中的 ProductAdditionContext.php 文件。将文件内的类名更改为 ProductAdditionContext。这个过程足以让 Behat 将此新类显示为目标选项吗？不，因为 Behat 的默认行为是仅搜索 FeatureContext 类。如果你不相信我，现在就尝试运行 Behat。你会发现 ProductAdditionContext 会被忽略。

要更改 Behat 的行为，我们必须创建一个配置文件。在 features 文件夹中创建一个名为 behat.yml 的文件，内容如下：

# behat.yml

```
default:
  autoload:
    '' : '%paths.base%/bootstrap'
  suites:
    default:
      paths:
        - '%paths.base%'
      contexts:
        - ProductAdditionContext
```

Behat 支持多个配置配置文件。我们将仅使用默认配置文件。autoload 键定义 Behat 查找上下文类的位置。suites 键允许定义多个上下文类套件集。在此情况下，我们只有一个默认的上下文套件集。paths 键定义将搜索用户故事文件的位置。

我们需要更改 **Behat 运行** 外部工具，具体是 **参数** 字段。将当前内容替换为以下代码：

```
--config ${project_loc}/features/behat.yml
```

现在，你可以运行 Behat 外部工具，输出将如下所示：

```
[0] None
[1] ProductAdditionContext
```

你不需要删除 FeatureContext.php。事实上，此文件是一个模板。你可以直接用它来学习，但此类不得用于实际上下文，除非你有一个名为 feature 的用户故事，这非常奇怪。好了，让我们回到 **控制台** 选项卡。控制台正在等待你的选择 – 0 或 1？键入 1，即 ProductAdditionContext。结果将如下所示：

```
/**
 * @Given there is a product called :arg1, which costs
 * $:arg2
 */
public function thereIsAProductCalledWhichCosts($arg1, $arg2)
{
    throw new PendingException();
}

/**
 * @When I add this product to the registration
 */
public function iAddThisProductToTheRegistration($arg1)
{
    throw new PendingException();
}

/**
 * @Then I should have a product called :arg1 in the
 * table products
 */
public function iShouldHaveProductInTheTableProducts($arg1)
{
    throw new PendingException();
}

/**
 * @Then the overall product price should be $:arg1
 */
public function theOverallProductPriceShouldBe($arg1)
{
    throw new PendingException();
}
```

请注意，生成的源代码具有名称与“在注册中插入产品”场景短语相同的方法。另请注意，文件中的值已在 PHP 源代码中替换为诸如 $arg1 之类的变量。我们将在后面了解这有何用处。但是，如果你期望看到生成的代码片段插入到 ProductAdditionContext 类中，你会发现该代码不在那里。

是的，Behat run 外部工具仅显示从用户故事生成的源代码。要将此代码添加到上下文类中，我们需要运行 Behat append snippets 外部工具。此外部工具也需要使用自定义配置，因此请按以下方式更改 **参数** 字段的值：

**参数:** --append-snippets --config ${project_loc}/features/behat.yml

还在等什么？运行它！此命令也会询问你想要哪个上下文。为 ProductAdditionContext 键入 1，你将看到以下输出：

```
u bootstrap/ProductAdditionContext.php - 'there is a product called "Troop Power Battery", which costs $2814' definition added
u bootstrap/ProductAdditionContext.php - 'I add the "Troop Power Battery" to the registration' definition added
u bootstrap/ProductAdditionContext.php - 'I should have a product called "Troop Power Battery" in the table products' definition added
u bootstrap/ProductAdditionContext.php - 'the overall product price should be $2814' definition added
```

你可以打开 ProductAdditionContext.php 文件，检查代码片段是否已插入其中。这意味着我们现在可以用 Behat 运行行为测试了吗？不。如果你现在运行 Behat 运行，你将看到以下输出：

```
Scenario: Inserting a product in the
registration            # ProductAddition.
feature:11
    Given there is a product called "Troop
    Power Battery", which costs $2814 # ProductAdditionContext::thereIsAProductCalledWhichCosts()
    TODO: write pending definition
    When I add this product to the
    registration    # ProductAdditionContext::iAddThisProductToTheRegistration()
    Then I should have a product called "Troop Power Battery" in the table products # ProductAdditionContext::iShouldHaveProductInTheTableProducts()
    And the overall product price should be $2814 # ProductAdditionContext::theOverallProductPriceShouldBe()
1 scenario (1 pending)
4 steps (1 pending, 3 skipped)
0m0.01s (9.31Mb)
```

现在，场景的短语在注释中带有上下文类中对应方法的名称。最终，我们可以看到该场景不再是未定义的，而是挂起的。步骤已更改为 1 个挂起和 3 个跳过。挂起状态意味着上下文类具有实现场景步骤的方法；但是，这些方法尚无内容。这就像 PHPUnit 测试用例因没有可测试的代码而失败一样。

我们必须完成场景实现。为此，我们需要实现与场景相关的模型和对象关系映射器。我们将在下一节开始此工作。

## 从用户故事创建模型

在语法课上，你可能听过这样的问题：“在这个短语中，主语是谁或是什么？”同样，我们提出以下问题：“在‘在注册中插入产品’场景中，模型是什么？”是的，没错——就是产品！然后，我们需要在行为测试类中实例化 Product 类。为此，我们必须在 ProductAdditionContext 类中定义一个 $product 属性：

```
private ?Product $product = null;
```

我们可以在 ProductAdditionContext 类的构造函数中实例化 Product 类，并在 thereIsAProductCalledWhichCosts 方法中使用此实例，如下所示：

```
public function __construct()
{
    $this->product = new Product();
}

/**
 * @Given there is a product called :arg1, which costs
 * $:arg2
 */
public function thereIsAProductCalledWhichCosts($arg1, $arg2)
{
    $data = [
        'name' => $arg1,
        'price' => $arg2
    ];

    $this->product->exchangeArray($data);
}
```

请注意，我们将 $arg1（产品名称）和 $arg2（产品价格）作为参数传递给 Product 类的 exchangeArray 方法。当然，如果你现在尝试运行 **Behat 运行**，将会发生失败，因为 Product 类尚不存在。我们处于 TDD 的第一步——一个失败的测试。请记住，BDD 是 TDD 的扩展，因此我们在 BDD 中保留 TDD 步骤。如果测试失败，我们就欠债了。我们需要编写代码以使测试成功运行。让我们创建 Product 模型。

> 注意 – 定义中的 Behat 参数
>
> feature 文件中被标识为参数的值借助 @Given、@When 和 @Then 注释映射到上下文类。这些注释告知以下方法是否带有参数。如果你更改定义中参数的值，则无需更改实现。例如，如果在 productaddition.feature 文件中更改了产品价格，则无需在 ProductAdditionContext 类中更改任何内容。这样可以避免在规则中的某个值发生变化时需要更改类。

Product 类将位于 Inventory 模块内。在此模块的 src 文件夹中创建一个 Model 文件夹，用于存放模型类。Product 模型的源代码将如下所示：

