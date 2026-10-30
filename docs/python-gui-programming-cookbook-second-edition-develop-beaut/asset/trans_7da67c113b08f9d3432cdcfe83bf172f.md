
当我们现在点击我们的 New York 按钮时，根据所选的日志级别，写入日志文件的内容会有所不同。默认日志级别是 DEBUG，这意味着所有内容都会写入日志：

```
GUI_Refactored.log
2016-12-19 18:30:40    *** Starting Test ***
2016-12-19 18:30:40    Test message
2016-12-19 18:30:42    2016-12-20 02:30:42 UTC+0000
2016-12-19 18:30:42    2016-12-19 18:30:42 PST-0800
2016-12-19 18:30:42    2016-12-19 21:30:42 EST-0500
```

当我们更改日志级别时，就可以控制写入日志的内容。我们通过调用 Logger 类的 setLoggingLevel 方法来实现这一点：

```
#-------------------------------------------------------------------------------
def setLoggingLevel(self, level):
    '''change logging level in the middle of a test.'''
    self.loggingLevel = level
```

在我们 GUI 的 __main__ 部分，我们将日志级别改为 MINIMUM，这样写入日志文件的输出就会减少：

```
if __name__ == '__main__':
#========================
# Start GUI
#========================
oop = OOP()
    oop.log.setLoggingLevel(oop.level.MINIMUM)
    oop.log.writeToLog('Test message')
    oop.win.mainloop()
```

现在，我们的日志文件不再显示 Test Message，只显示满足所设置日志级别的消息：

```
GUI_Refactored.log
2016-12-19 18:34:42    *** Starting Test ***
2016-12-19 18:34:43    2016-12-20 02:34:43 UTC+0000
```

## 工作原理...

在本节中，我们充分利用了 Python 内置的 __main__ 自测部分。我们引入了自己的日志文件，并同时学习了如何创建不同的日志级别。通过这样做，我们完全控制了写入日志文件的内容。

## 使用单元测试创建健壮的 GUI

Python 自带一个内置的单元测试框架，在本节中，我们将开始使用该框架来测试我们的 Python GUI 代码。

在开始编写单元测试之前，我们需要设计测试策略。我们可以轻松地将单元测试与被测代码混合在一起，但更好的策略是将应用程序代码与单元测试代码分开。

![](img/986b0dbf3f539f4f3baa82bd3003f04e_317_0.png)

PyUnit 是根据所有其他 xUnit 测试框架的原则设计的。

## 准备工作

我们将测试本章前面创建的国际化的 GUI。

## 如何实现...

为了使用 Python 内置的单元测试框架，我们必须导入 Python 的 unittest 模块。让我们创建一个新模块并将其命名为 UnitTests.py。

我们首先导入 unittest 模块，然后创建自己的类，并在此类中继承和扩展 unittest.TestCase 类。

最简单的代码如下所示：

```
import unittest

class GuiUnitTests(unittest.TestCase):
    pass

if __name__ == '__main__':
```

unittest.main()

这段代码目前还没有做太多事情，但当我们运行它时，不会出现任何错误，这是一个好迹象：

UnitTestsMinimum.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_318_0.png)

实际上，我们确实会得到一段写入控制台的输出，它说明我们成功运行了零个测试……

这个输出有点误导性，因为到目前为止，我们所做的只是创建了一个不包含任何实际测试方法的类。

我们通过遵循默认命名约定（所有测试方法都以单词 test 开头）来添加实际执行单元测试的测试方法。这是一个可以更改的选项，但遵循此命名约定更容易、更清晰。

让我们添加一个测试方法，用来测试我们 GUI 的标题。这将验证在传入预期参数的情况下，我们是否得到预期结果：

```
```python
import unittest
from Ch08_Code.LanguageResources import I18N

class GuiUnitTests(unittest.TestCase):

    def test_TitleEnglish(self):
        i18n = I18N('en')
        self.assertEqual(i18n.title, "Python Graphical User Interface")
```
```

我们正在从 Resources.py 模块导入 I18N 类，并传入英语作为 GUI 中显示的语言。由于这是我们的第一个单元测试，我们还会打印出标题结果，以确保我们知道返回的是什么。接下来，我们使用 unittest 的 assertEqual 方法来验证标题是否正确。

运行这段代码会得到 OK，这意味着单元测试通过：

UnitTests_One.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_319_0.png)

单元测试运行并成功，这由一个点号和 OK 字样表示。如果测试失败或出错，我们不会得到点号，而是会得到 F 或 E 作为输出。

现在，我们可以通过验证 GUI 德语版本的标题来执行同样的自动化单元测试检查。我们只需复制、粘贴并修改代码：

```
import unittest
from Ch08_Code.LanguageResources import I18N

class GuiUnitTests(unittest.TestCase):

    def test_TitleIsEnglish(self):
        i18n = I18N('en')
        self.assertEqual(i18n.title, "Python Graphical User Interface")

    def test_TitleIsGerman(self):
        i18n = I18N('en')
        self.assertEqual(i18n.title,
            'Python Grafische Benutzeroberfl' + "u00E4" + 'che')
```

现在，我们用两种语言测试我们国际化的 GUI 标题，并在运行代码后得到以下结果：

![](img/986b0dbf3f539f4f3baa82bd3003f04e_320_0.png)

我们运行了两个单元测试，但得到的不是 OK，而是失败。发生了什么？

对于 GUI 的德语版本，我们的断言失败了……

在调试代码时，我们发现，在复制、粘贴和修改单元测试代码的过程中，我们忘记将德语作为语言参数传入。我们很容易修复这个问题：

```
def test_TitleIsGerman(self):
    # i18n = I18N('en')      # <= Bug in Unit Test
    i18n = I18N('de')
    self.assertEqual(i18n.title,
        'Python Grafische Benutzeroberfl' + "u00E4" + 'che')
```

当我们重新运行单元测试时，所有测试都通过了，得到了预期的结果：

已修正失败的 UnitTestsFail.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_320_1.png)

单元测试代码也是代码，也可能有 bug。

虽然编写单元测试的目的是真正测试应用程序代码，但我们必须确保测试编写正确。来自**测试驱动开发 (TDD)** 方法论的一种方法可能会帮助我们。

在 TDD 中，我们在实际编写应用程序代码之前先编写单元测试。现在，如果一个测试对于一个根本不存在的方法也能通过，那就说明有问题。下一步是创建那个不存在的方法，并确保它会失败。之后，我们可以编写使单元测试通过所需的最少量代码。

## 工作原理…

在本节中，我们开始测试我们的 Python GUI，并用 Python 编写单元测试。我们看到，Python 单元测试代码也只是代码，可能包含需要修正的错误。在下一节中，我们将扩展本节的代码，并使用 Eclipse IDE 的 PyDev 插件自带的图形化单元测试运行器。

## 如何使用 Eclipse PyDev IDE 编写单元测试

在上一节中，我们开始使用 Python 的单元测试功能，在本节中，我们将进一步使用该功能来确保 GUI 代码的质量。

我们会对 GUI 进行单元测试，以确保 GUI 显示的国际化字符串符合预期。

在上一节中，我们在单元测试代码中遇到了一些 bug，但通常情况下，单元测试会发现因修改现有应用程序代码而导致的回归 bug，而不是单元测试代码本身的 bug。一旦我们确认单元测试代码是正确的，通常就不会再更改它。

我们的单元测试也可以作为代码预期行为的文档。

默认情况下，Python 单元测试使用文本单元测试运行器执行，我们可以在 Eclipse IDE 中的 PyDev 插件中运行它。我们也可以在控制台窗口中运行完全相同的单元测试。

除了本节中的文本运行器之外，我们还将探索 PyDev 的图形化单元测试功能，该功能可以在 Eclipse IDE 中使用。

## 准备工作

我们将扩展上一节中开始使用 Python 单元测试的内容。

## 如何实现…

Python 单元测试框架带有所谓的 **测试夹具（Fixtures）**。

关于什么是测试夹具，请参阅以下 URL：

- `https://docs.python.org/3.6/library/unittest.html`
- `https://en.wikipedia.org/wiki/Test_fixture`
- `http://www.boost.org/doc/libs/1_51_0/libs/test/doc/html/utf/user-guide/fixture.html`

这意味着我们可以创建 `setup()` 和 `teardown()` 单元测试方法，这样在执行任何单个测试之前，都会在开始时调用 `setup()` 方法，而在每个单元测试结束时，都会调用 `teardown()` 方法。

这种测试夹具功能为我们提供了一个非常可控的环境，我们可以在其中运行单元测试。这类似于使用前置条件和后置条件。

让我们设置单元测试环境。我们将创建一个新的测试类，专注于验证上述代码的正确性：

```
import unittest
from Ch08_Code.LanguageResources import I18N
```

