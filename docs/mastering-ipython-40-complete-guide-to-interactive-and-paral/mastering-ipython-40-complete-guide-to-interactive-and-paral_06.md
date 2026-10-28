## nose2

nose 测试框架最初是 pytest 的一个克隆，当时 pytest 还处于 0.8 版本。它的标语是“nose extends unittest to make testing easier”（见 http://nose.readthedocs.org/en/latest/）。然而，所有美好的事物都有终结；目前 nose 已处于维护模式，而且已经持续了数年。

由于 unittest 仅适用于 Python 2.7 及以上版本，因此创建了 unittest2 作为早期版本的向后移植。除了 unittest 的功能外，unittest2 还包含改进的 API 和更好的断言。实际上，unittest2 不仅是向后移植，也是对 unittest 的一次修订。

对 unittest 的更改非常重要，以至于 nose 的继任者——颇具创意地命名为 nose2——基于 unittest2 的插件分支构建。其主页上写道：

> *nose2 是基于 unittest2 插件分支的下一代 Python 测试工具。nose2 旨在改进 nose，具体方式如下：*
>
> - *提供更好的插件 API*
> - *更方便用户配置*
> - *简化内部接口和流程*
> - *在不进行转换的情况下，从同一代码库同时支持 Python 2 和 Python 3*

### 运行单个测试

可以通过在命令行中指定模块名称来运行单个测试：

```text
(my_rpy2_zone)-bash-4.1$ ~/.local/bin/nose2 -v test1
test_f (test1.TestHailStones) ... 1
FAIL

===============================================================
FAIL: test_f (test1.TestHailStones)
---------------------------------------------------------------
Traceback (most recent call last):
  File "/nfs/02/wit0096/Packt/chap08/test1.py", line 10, in test_f
    self.assertEqual(f(i), ans[i])
AssertionError: 1 != 0

---------------------------------------------------------------
Ran 1 test in 0.000s

FAILED (failures=1)
```

不要指定完整的文件名，否则会产生一条没有帮助的错误信息：

```text
(my_rpy2_zone)-bash-4.1$ ~/.local/bin/nose2 -v test1.py
test1.py (nose2.loader.LoadTestsFailure) ... ERROR

===============================================================
ERROR: test1.py (nose2.loader.LoadTestsFailure)
---------------------------------------------------------------
Traceback (most recent call last):
  File "/nfs/02/kmanalo/envs/my_rpy2_zone/lib/python3.4/unittest/case.py", line 58, in testPartExecutor
    yield
  File "/nfs/02/kmanalo/envs/my_rpy2_zone/lib/python3.4/unittest/case.py", line 580, in run
    testMethod()
  File "/nfs/02/wit0096/.local/lib/python3.4/site-packages/nose2/loader.py", line 120, in testFailure
    raise exception
  File "/nfs/02/wit0096/.local/lib/python3.4/site-packages/nose2/plugins/loader/parameters.py", line 118, in loadTestsFromName
    result = util.test_from_name(name, module)
  File "/nfs/02/wit0096/.local/lib/python3.4/site-packages/nose2/util.py", line 106, in test_from_name
    parent, obj = object_from_name(name, module)
  File "/nfs/02/wit0096/.local/lib/python3.4/site-packages/nose2/util.py", line 128, in object_from_name
    parent, obj = obj, getattr(obj, part)
AttributeError: 'module' object has no attribute 'py'
```

```text
Ran 1 test in 0.000s
```

```text
FAILED (errors=1)
```

nose2 会将 `.py` 部分理解为指定模块名的一部分。这在指定特定模块时可能很有用，但在其他情况下可能会导致出人意料的结果。

### 断言、设置和拆除

与 pytest 一样，nose2 也使用 Python 中的 assert 语句。它不使用与 pytest 相同的 fixture 风格，但确实支持一种经过修改的经典 xUnit 风格设置和拆除，以及其自己的一套 fixture。

> 目前，nose2 的测试功能在使用测试类时存在一些小问题。后面的示例将使用模块中包含的单个测试函数，但其他方面应与此前的示例类似。

#### 改进的 xUnit 风格

nose2 没有采用仅靠函数名区分的简单 setup 和 teardown 函数，而是允许在函数上设置 setup 和 teardown 属性。我们可以将第一个随机数生成测试改写如下：

```python
import numpy as np
import myrand
import scipy.stats
```


```python
import sys
import random

numVals = 0
vals = 0
randGen = 0

def setup():
    print("Doing setUp")
    global numVals
    global vals
    global randGen
    numVals = 10000
    vals = np.zeros((10), dtype=np.int32)
    randGen = myrand.MyRand()

def test_bad():
    print("Doing test_bad")
    x0 = 15
    p1 = 50
    p2 = 100
    modulus = 2217
    randGen.set(p1, p2, x0, modulus)
    for i in range(numVals):
        tmp = randGen.next()
        tmp = tmp % 10
        vals[tmp] = vals[tmp] + 1

    chi2, p = scipy.stats.chisquare(vals)
    assert p > 0.05

def test_better():
    print("Doing test_better")
    x0 = 79
    p1 = 263
    p2 = 71
    modulus = sys.maxsize
    randGen.set(p1, p2, x0, modulus)
    for i in range(numVals):
        tmp = randGen.next()
        tmp = tmp % 10
        vals[tmp] = vals[tmp] + 1

    chi2, p = scipy.stats.chisquare(vals)
```

```text
testrand_nose2_1.FunctionTestCase (test_better) ... Doing setUp
Doing test_better
Doing tearDown
ok
testrand_nose2_1.FunctionTestCase (test_builtin) ... Doing setUp
Doing test_builtin
Doing tearDown
ok

==========================================================================
FAIL: testrand_nose2_1.FunctionTestCase (test_bad)
--------------------------------------------------------------------------
Traceback (most recent call last):
  File "/nfs/02/wit0096/Packt/chap08/testrand_nose2_1.py", line 33, in test_bad
    assert p > 0.05
AssertionError

--------------------------------------------------------------------------
Ran 3 tests in 0.184s

FAILED (failures=1)
```

### 使用装饰器

nose2 在 `nose2.tools.decorators` 模块中提供了两个装饰器，它们对 `setup` 和 `teardown` 很有帮助：

| 装饰器 | 效果 |
|---|---|
| `with_setup(setup)` | 在每个测试之前运行 setup 函数 |
| `with_teardown(teardown)` | 在每个测试之后运行 teardown 函数 |

这使我们能够将之前的测试重写如下：

```python
import numpy as np
import myrand
import scipy.stats
import sys
import random
import nose2.tools.decorators

numVals = 0
vals = 0
randGen = 0

def setup():
    print("Doing setUp")
    global numVals
    global vals
    global randGen
    numVals = 10000
    vals = np.zeros((10), dtype=np.int32)
    randGen = myrand.MyRand()

def teardown():
    print("Doing tearDown")
    randGen.reset()

@nose2.tools.decorators.with_setup(setup)
@nose2.tools.decorators.with_teardown(teardown)
def test_bad():
    print("Doing test_bad")
    x0 = 15
    p1 = 50
    p2 = 100
    modulus = 2217
    randGen.set(p1, p2, x0, modulus)
    for i in range(numVals):
        tmp = randGen.next()
        tmp = tmp % 10
        vals[tmp] = vals[tmp] + 1

    chi2, p = scipy.stats.chisquare(vals)
    assert p > 0.05

@nose2.tools.decorators.with_setup(setup)
@nose2.tools.decorators.with_teardown(teardown)
def test_better():
    print("Doing test_better")
    x0 = 79
    p1 = 263
    p2 = 71
    modulus = sys.maxsize
    randGen.set(p1, p2, x0, modulus)
    for i in range(numVals):
        tmp = randGen.next()
        tmp = tmp % 10
        vals[tmp] = vals[tmp] + 1

    chi2, p = scipy.stats.chisquare(vals)
    assert p > 0.05

@nose2.tools.decorators.with_setup(setup)
@nose2.tools.decorators.with_teardown(teardown)
def test_builtin():
    print("Doing test_builtin")
    for i in range(numVals):
        tmp = random.randint(0, 9)
        vals[tmp] = vals[tmp] + 1

    chi2, p = scipy.stats.chisquare(vals)
    assert p > 0.05
```

预期结果如下：

```text
(my_rpy2_zone)-bash-4.1$ ./local/bin/nose2 -v testrand_nose2decs_1
testrand_nose2decs_1.FunctionTestCase (test_bad) ... Doing setUp
Doing test_bad
Doing tearDown
FAIL
testrand_nose2decs_1.FunctionTestCase (test_better) ... Doing setUp
Doing test_better
Doing tearDown
ok
testrand_nose2decs_1.FunctionTestCase (test_builtin) ... Doing setUp
Doing test_builtin
Doing tearDown
ok
======================================================================
FAIL: testrand_nose2decs_1.FunctionTestCase (test_bad)
----------------------------------------------------------------------
Traceback (most recent call last):
    File "/nfs/02/wit0096/Packt/chap08/testrand_nose2decs_1.py", line 40,
    in test_bad
        assert p > 0.05
AssertionError

------------------------------------------------------------
Ran 3 tests in 0.277s

FAILED (failures=1)
```

### 插件

从 nose 迁移到 nose2 的一个重要特性是实现了改进的插件 API。插件提供了额外的功能，并允许第三方扩展 nose2。列出所有插件的完整清单超出了本书的范围，而且在出版时无论如何都难免不完整。一个示例就足以证明插件的强大功能。

#### 使用 junitxml 插件生成 XML

尽管 nose2 默认将测试结果输出到 stdout，但当测试套件很大时，一个人盯着屏幕观察结果滚动可能过于吃力。在这种情况下，让每个测试以标准化格式生成输出会很有帮助。xUnit 家族已经普及了 XML 作为这种格式，因此许多测试报告工具都期望以 XML 作为输入。

为了输出 XML，可以按如下方式调用 junitxml 插件：

```bash
(my_rpy2_zone)-bash-4.1$ ~/.local/bin/nose2 -v --plugin nose2.plugins.junitxml --junit-xml testrand_nose2decs_1
```

输出由两个流组成：

- 通常的输出，发送到 stdout
- 一个 XML 文件（nose2-junit.xml），创建于测试运行所在目录

### 小结

一个运行中的程序最重要的属性就是正确性。很多时候，测试被留到开发结束时才进行，而到了那时，由于项目已经延期，测试要么被完全跳过，要么草草了事。对于科学和工程代码来说尤其如此，因为可能危及生命。即使是探索性代码也需要经过测试，以便基于它做出合理的决策。没有相关单元测试的代码不应被接受到项目仓库中。因此，应尽可能让开发者轻松地进行测试。

虽然 IPython 支持许多出色的可视化工具，可以为结果提供视觉反馈，但没有任何东西可以替代彻底的单元测试。本章概述了单元测试的基础知识——setup、test 和 teardown——并展示了三个不同框架如何实现这些概念。使用哪种框架的决定权在项目团队手中。

在下一章中，我们将探讨对专业级软件中另一个常被忽视的组件的支持：文档。我们将讨论文档面向的不同受众、每个受众所关注的信息类型、格式选项，以及可用于让文档编写相对轻松的工具。
