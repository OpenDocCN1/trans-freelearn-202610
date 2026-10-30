

# 编写可维护的测试

在为应用程序代码编写测试时，我们会遇到这样的场景：应用程序代码逻辑的任何更新都会破坏单元/集成测试。这是不可避免的，这也是一个好迹象，说明你编写了良好的测试用例，当代码的预期行为发生变化时测试会失败，从而使测试用例失败。然而，随着项目的成熟，这会成为瓶颈，有时我们可能会发现更新应用程序代码只需要一个小时，但修复测试用例可能需要整整一天。这时你需要退一步重新评估你的测试用例以及开发者如何编写测试用例。没有银弹可以修复这个问题，避免编写脆弱的测试需要经验。然而，有一些指导原则可以帮助你改进：

* **编写更多单元测试**：通过编写更多检查核心业务逻辑的单元测试，开发者将能够轻松识别失败的测试用例及其原因。这将帮助他们更快地修复。
* **使用动态数据**：使用动态数据有助于使测试用例更灵活。例如，使用 `factory_boy` 从返回值断言模型对象始终是首选。*请记住，测试的动态数据是一把双刃剑，如果没有适当实现，它会造成很多损害*。
* **避免业务逻辑耦合**：我们应该编写仅测试函数/方法输出的测试；如果你开始测试实现细节和业务逻辑，那么即使输出契约保持一致，你的测试也注定会在每次业务逻辑更新时失败。
* **创建小型测试用例**：添加检查函数/方法的多个用例的测试用例很诱人，但这会导致无法识别哪个测试用例失败的问题，并且需要太多更新来识别和解决失败的测试用例。

这些是主观观点，应仅作为建议/指导使用；每个开发者对其系统有更好的了解，应该做出更好的判断来改进编写更好的测试。

## 在测试中使用反向函数进行 URL 路径

在 Django/DRF 中编写测试时，我们必须使用 URL 来访问我们的视图和 API。直接使用定义的 URL 可能很诱人，但建议使用 `django.urls` 包中的 `reverse` 函数来拥有能够管理 URL 路径任何更新的健壮测试用例。`reverse` 函数负责创建动态 URL，这可以帮助你专注于编写业务逻辑测试用例，而不必担心创建 URL。这也是一把双刃剑，如果为遗留代码更新 URL 路径可能会导致失败，但此类更改应在代码到达生产环境之前在代码审查期间被标记。

## 使用身份验证测试

你可以在 DRF/Django 中的任何 API 轻松添加身份验证；同样，任何开发者都可以轻松地从 API 中移除身份验证，因此强烈建议为每个 API 编写身份验证验证测试用例。我们之前讨论过如何在 *Testing API authentication* 部分为身份验证编写测试用例。务必确保编写至少一个测试来验证 API 的 *基于令牌* 或其他任何身份验证系统。例如，编写一个测试将利用 `self.client.credentials` 方法进行登录请求。

## 使用测试标签对测试进行分组

随着项目的成熟，我们将拥有快速、慢速、核心或某些不同类型的测试用例。可能无法在本地或开发阶段运行所有测试，因此我们可能希望对测试进行分组以执行/跳过它们。我们可以将不同的测试用例分组到不同的段中，并根据需要运行它们。Django 提供了一个简单的装饰器接口，可以帮助我们将类似的测试用例标记为一组。例如，在以下代码中，我们将 `SampleTestCase` 中的不同测试标记为 `fast`、`slow` 或 `core`。这可以帮助我们一起执行所有 `core` 测试用例或 `fast` 测试用例。将以下代码添加到 `blog/tests.py` 文件中：

```
from django.test import tag
from rest_framework.test import APITestCase
class BlogTests8(APITestCase):
    @tag("fast")
    def test_fast(self):
        print("fast test running")
    @tag("slow")
    def test_slow(self):
        print("slow test running")
    @tag("slow", "core")
    def test_slow_but_core(self):
        print("slow but core test running")
```

要运行特定组的标记测试用例，我们可以使用以下命令：

```
python manage.py test --tag=core
```

此命令将只运行所有标记为 `core` 标签的测试用例——在我们的例子中是 `test_slow_but_core`。有关如何高效使用测试用例的 `tag` 概念的更多详细信息，请查看 Django 的官方文档：[https://docs.djangoproject.com/en/stable/topics/testing/tools/#tagging-tests](https://docs.djangoproject.com/en/stable/topics/testing/tools/#tagging-tests)。

## 使用 Postman 创建集成测试套件

Postman 是一个验证 API 契约的绝佳工具。然而，很少有开发者利用 Postman 测试脚本的强大功能来编写 E2E 测试。我们可以编写 E2E 测试来验证每个 API 的契约。这可以通过编写测试脚本来实现。一旦我们有了测试脚本，我们就可以编写监控器来持续测试结果。这样，我们可以在任何预生产环境中设置 E2E 测试设置，并在任何测试失败时收到通知。以下是一些可以帮助你开始编写 Postman E2E 测试的重要链接：

* 如何在 Postman 中编写测试脚本：[https://learning.postman.com/docs/writing-scripts/script-references/test-examples/](https://learning.postman.com/docs/writing-scripts/script-references/test-examples/)
* 如何创建监视器以定期检查测试是否通过：[https://learning.postman.com/docs/monitoring-your-api/intro-monitors/](https://learning.postman.com/docs/monitoring-your-api/intro-monitors/)
* 如何使用 Postman CLI 从 CI/CD 流水线运行测试用例：[https://learning.postman.com/docs/postman-cli/postman-cli-overview/](https://learning.postman.com/docs/postman-cli/postman-cli-overview/)

编写 Postman 测试脚本并通过 Postman 监控器定期运行它们，是为你的 API 设置任何测试套件的最简单、最快的方法之一。你也可以使用市场上可用的任何其他工具来实现此目的——例如，Insomnia 提供了这样的测试界面（[https://docs.insomnia.rest/insomnia/unit-testing](https://docs.insomnia.rest/insomnia/unit-testing)）。

## 创建不同类型的测试

为应用程序代码编写测试是开发项目的一部分，团队应始终计算编写测试用例所需的时间，然后提供项目的最终估算。除了编写单元测试和集成测试外，我们还应编写一些系统级测试，以帮助维护代码质量和系统的良好健康状况：

* **覆盖率阈值**：我们应为每个 `Pull Request` (`PR`) 设置代码覆盖率阈值。每当提出的 `PR` 降低整体覆盖率时，我们应将 CI 流水线标记为失败的测试。

