## unittest

unittest 是 Python 内置（自 2.1 版本起）的测试框架。它仿照优秀的 Java JUnit 框架以及一般的 xUnit 测试框架而设计。这造就了一种简洁、面向对象的风格。

### 重要概念

unittest 以直接的方式支持标准的单元测试组成部分。以下概念与功能元素相对应：

- **测试用例：** 这是一个单独的测试或一组测试。一个测试用例中的所有测试都属于同一个类，并共享一个夹具（fixture）。
- **测试夹具：** 为一个类中的所有测试用例执行 setUp 和 tearDown 所需的函数。
- **测试套件：** 这是测试用例的集合。测试套件中的测试用例旨在一起运行。
- **测试运行器：** 这是一个运行测试套件并向用户报告结果的组件。

让我们更仔细地看前面的一些代码作为示例：

```
def F(n):
    curr = n
    tmp = 1
    while curr != 1:
        tmp = tmp + 1
        if curr % 2 == 1:
```

```
244      self.result = testRunner.run(self.test)
245      if self.exit:
--> 246      sys.exit(not self.result.wasSuccessful())
247
248 main = TestProgram

SystemExit: True
```

在这种情况下，这并不是很有趣，但如果有需要，信息就在那里。

一个小修复应该能解决这个问题：

```
def f(n):
    curr = n
    tmp = 0
    while curr != 1:
        tmp = tmp + 1
        if curr % 2 == 1:
            curr = 3 * curr + 1
        else:
            curr = curr/2
    return tmp
```

hail2.py

同时相应修改测试（通常只需修改原始源代码，但为了教学目的，这样更清晰）：

```
import unittest
from hail2 import f

class TestHailStones(unittest.TestCase):

    def test_f(self):
        ans = [0, 0, 1, 7, 2, 5, 8, 16, 3, 19, 6]
        for i in range(1, 11):
            print(i)
            self.assertEqual(f(i), ans[i])

if __name__ == '__main__':
    unittest.main()
```

test2.py

输出将变为以下内容：

```
In [2]: %run testrand.py
Doing setUp
Doing test_bad
Doing tearDown
Doing setUp
Doing test_better
Doing tearDown
.Doing setUp
Doing test_builtin
Doing tearDown
.
======================================================================
FAIL: test_bad (__main__.TestRandoms)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/nfs/02/wit0096/Packt/chap08/testrand.py", line 29, in test_bad
    self.assertGreater(p, 0.05)
AssertionError: 3.5277031043732684e-204 not greater than 0.05
----------------------------------------------------------------------
Ran 3 tests in 0.171s

FAILED (failures=1)
An exception has occurred, use %tb to see the full traceback.

SystemExit: True
```

关于整个程序，有一些值得指出的地方：

- 代码包含多个测试。无需将它们标记为测试，因为 unittest 假定每个以 test 开头的函数都是测试，并会将其作为测试运行。

```
Doing tearDown
.Doing setUp
Doing test_better
Doing tearDown
.Doing setUp
Doing test_builtin
Doing tearDown
.
------------------------------------------------------------
Ran 3 tests in 0.227s

OK
```

### 装饰器

对一个测试使用 `assertLess`，对另一个测试使用 `assertGreater` 有点笨拙。这种小的不一致很容易被忽视，从而在以后导致更大的问题。更糟糕的是，它可能会被发现并“纠正”，但纠正反而会导致测试失败，并让人困惑地站在那里，想不明白当初为什么要那样写。

这种事情经常发生，尤其是当测试不是先于代码编写时。

> **测试驱动开发**
>
> **测试驱动开发（TDD）** 是一种软件开发过程，在这种过程中，测试先于代码编写。这个过程允许测试用作自动化手段，用来检查代码是否满足（至少部分）需求。测试还可以用来确保将来对代码的修改不会破坏任何已有的功能。

理论上，在代码被接受之前，所有测试都应该运行并通过。但这并不总是可行的。为此，unittest 提供了装饰器，允许测试人员显式标记失败和跳过的测试。

例如，我们之前的测试可以如下进行装饰：

```
@unittest.expectedFailure
def test_bad(self):
    print("Doing test_bad")
    x0 = 15
    p1 = 50
```



从命令行调用它非常容易：

```
(my_rpy2_zone)-bash-4.1$ py.test
======================================= test session starts ===
========================================
platform linux -- Python 3.4.4, pytest-2.8.7, py-1.4.31, pluggy-0.3.1
rootdir: .../chap08, inifile:
collected 0 items

======================================= no tests ran in 0.03 seconds ========================================
```

pytest 会尝试根据将在后面章节介绍的规则自动定位测试。在这种情况下，没有找到任何测试。

## 向后兼容性

把之前章节中对测试所做的所有工作都丢弃似乎很可惜。pytest 会在形如 test_*.py（和 *_test.py）的文件中查找测试。最懒的做法就是直接把 test1.py 复制成 test_1.py 看看会发生什么：

```
(my_rpy2_zone)-bash-4.1$ cp test1.py test_1.py
(my_rpy2_zone)-bash-4.1$ py.test
======================================= test session starts ===
========================================
platform linux -- Python 3.4.4, pytest-2.8.7, py-1.4.31, pluggy-0.3.1
rootdir: .../chap08, inifile:
collected 1 items

test_1.py F

======================================= FAILURES =========
========================================
________________________________________ TestHailStones.test_f ________________________________________

self = <test_1.TestHailStones testMethod=test_f>

    def test_f(self):
```

- `recurse` 进入目录，除非它们匹配 `norecursedirs`。
- `test_*.py` 或 `*_test.py` 文件，按其测试包名称导入。
- 测试前缀的 test 类（没有 `__init__` 方法）。
- 以 `test_` 为前缀的测试函数或方法是测试项。

## 组织测试文件

pytest 允许测试函数与被测试的函数包含在同一文件中。这样做有一些优点，即更有可能在功能更新时更新测试，并且使测试易于查找。

然而，对于除最小项目之外的任何项目，这都不是一个好主意，原因有几个：

- 一个好的测试套件可能比被测试的代码大好几倍。把两者放在同一位置会掩盖代码。
- 通常情况下，不同的团队开发测试和代码。使用不同的文件可以使版本控制更轻松。
- 测试不应依赖于代码的编写方式，代码也不应依赖于测试的结构。确保这一点的最简单方法是让它们完全分开。

当测试不再与代码在同一文件中时，将它们保存在易于查找并与所测试代码关联的位置非常重要。并行目录结构是一种简单的方法来实现这一点：

```
pkg/
    __init__.py
    prog1.py
    …

tests/
    test_prog1.py
    …
```

注意 tests 目录中没有 `__init__.py` 文件。这为之前列出的测试发现规则之一提供了更轻松的处理。pytest 并没有要求必须完全镜像此结构；它只是一个有用的启发式方法。

## 断言

pytest 使用 Python 中的 assert 语句。这就避免了记住 unittest 中所有不同的 `assert*` 方法的需求。考虑一下两者在可读性上的差异：

```
self.assertEqual(f(i), ans[i])
```

与这行代码相比：

```
assert f(i) == ans[i]
```

总体而言，pytest 测试在代码行数上更少。`test2.py` 测试文件可以在 pytest 下重写如下：

```
from hail2 import f

class TestHailstones():

    def test_f(self):
        ans = [0, 0, 1, 7, 2, 5, 8, 16, 3, 19, 6]
        for i in range(1, 11):
            print(i)
            assert f(i) == ans[i]
```

test_2.py

以下是结果：

```
In [2]: !py.test test_2.py
================================================= test session starts ===
=================================================
platform linux -- Python 3.4.4, pytest-2.8.7, py-1.4.31, pluggy-0.3.1
rootdir: /nfs/02/wit0096/Packt/chap08, inifile:
collected 1 items

test_2.py .

================================================= 1 passed in 0.02 seconds
=================================================
```

甚至结果也可以通过 `-q` 命令行参数缩写：

```
In [3]: !py.test -q test_2.py
.
1 passed in 0.01 seconds
```

## 一个使用 setUp 和 tearDown 的测试

unittest 依赖于重载其父类的 setUp 和 tearDown 函数。由于 pytest 去掉了父类，它需要不同的机制。事实上，它有两个：经典的 xUnit 风格和一个使用 fixtures 的方法。

### 经典 xUnit 风格

提供以下方法进行设置和清理：

| 方法 | 效果 |
|---|---|
| setup_method(self, method) | 在类的每个测试方法之前调用 |
| teardown_method(self, method) | 在类的每个测试方法之后调用 |
| setup_function(function) | 在模块的每个测试方法之前调用 |
| teardown_function(function) | 在模块的每个测试方法之后调用 |

这使得重写我们第一个随机数测试变得轻而易举：

```
import numpy as np
import myrand
import scipy.stats
import sys
import random

class TestRandoms():

    def setup_method(self, mthd):
        print("Doing setUp")
        self.numVals = 10000
        self.vals = np.zeros((10), dtype=np.int32)
        self.randGen = myrand.MyRand()

    def test_bad(self):
        print("Doing test_bad")
        x0 = 15
        p1 = 50
        p2 = 100
        modulus = 2217
        self.randGen.set(p1, p2, x0, modulus)
        for i in range(self.numVals):
            tmp = self.randGen.next()
            tmp = tmp % 10
```

## 但在演示中它运行正常！ – 测试

```
collected 3 items

testrand_1.py F..

========================================================== FAILURES ========
==================================================
____________________________ TestRandoms.test_bad __

self = <testrand_1.TestRandoms object at 0x2b6610db1b38>

def test_bad(self):
    print("Doing test_bad")
    x0 = 15
    p1 = 50
    p2 = 100
    modulus = 2217
    self.randGen.set(p1, p2, x0, modulus)
    for i in range(self.numVals):
        tmp = self.randGen.next()
        tmp = tmp % 10
        self.vals[tmp] = self.vals[tmp] + 1

    chi2, p = scipy.stats.chisquare(self.vals)
>   assert p > 0.05
E   assert 3.5277031043732684e-204 > 0.05

testrand_1.py:28: AssertionError
------------------------------- Captured stdout setup
--------------------------------
Doing setUp
-------------------------------- Captured stdout call
--------------------------------
Doing test_bad
======================================== 1 failed, 2 passed in 1.16 seconds ========================================
```

```
E    assert 3.5277031043732684e-204 > 0.05

testrand_1.py:28: AssertionError
--------------------------------------------- Captured stdout setup
---------------------------------------------
Doing setUp
--------------------------------------------- Captured stdout call
---------------------------------------------
Doing test_bad
============================================= 1 failed, 2 passed in 0.82 seconds =============================================
```

## 使用 fixtures

一个函数可以被装饰为 pytest fixture。此 fixture 装饰器可以接受一个 "scope" 参数，指定 fixture 应该被调用的时间：

| Scope | 效果 |
|---|---|
| session | 函数在会话开始时执行一次 |
| module | 函数在模块开始时执行一次 |
| class | 函数在类开始时执行一次 |
| function | 函数在每个测试用例之前执行 |

我们的第二个随机数测试类然后可以重写如下：

```
import numpy as np
import myrand
import scipy.stats
import sys
import random
import pytest

class TestRandoms( ):

    @classmethod
    @pytest.fixture(scope="class")
    def setUpClass(cls):
        print("Doing setUpClass")
        cls.numVals = 10000

    @pytest.fixture(scope="function")
    def setUp(self, request):
```


```python
print("Doing setUp")
self.vals = np.zeros((10), dtype=np.int32)
self.randGen = myrand.MyRand( )
self.numVals = 10000
def tearDown(self):
    print("Doing tearDown")
    self.randGen.reset( )
request.addfinalizer(tearDown)

def test_bad(self, setUp, setUpClass):
    print("Doing test_bad")
    x0 = 15
    p1 = 50
    p2 = 100
    modulus = 2217
    self.randGen.set(p1, p2, x0, modulus)
    for i in range(self.numVals):
        tmp = self.randGen.next( )
        tmp = tmp % 10
        self.vals[tmp] = self.vals[tmp] + 1

    chi2, p = scipy.stats.chisquare(self.vals)
    assert p < 0.05

def test_better(self, setUp, setUpClass):
    print("Doing test_better")
    x0 = 79
    p1 = 263
    p2 = 71
    modulus = sys.maxsize
    self.randGen.set(p1, p2, x0, modulus)
    for i in range(self.numVals):
        tmp = self.randGen.next( )
        tmp = tmp % 10
        self.vals[tmp] = self.vals[tmp] + 1

    chi2, p = scipy.stats.chisquare(self.vals)
    assert p > 0.05

def test_builtin(self, setUp, setUpClass):
    print("Doing test_builtin")
    for i in range(self.numVals):
        tmp = random.randint(0, 9)
        self.vals[tmp] = self.vals[tmp] + 1

    chi2, p = scipy.stats.chisquare(self.vals)
    assert p > 0.05
```

testrand2.py

经过适当编辑（必须删除 addfinalizer 那一行——在我的环境中，它会导致 Python 出现内部错误 INTERNALERROR），程序可以运行并产生如下输出：

```text
In [27]: !py.test -s testrand_2.py
================================================================================ test session starts ===
================================================================================
platform linux -- Python 3.4.4, pytest-2.8.7, py-1.4.31, pluggy-0.3.1
rootdir: /nfs/02/wit0096/Packt/chap08, inifile:
collected 3 items

testrand_2.py Doing setUpClass
Doing setUp
Doing test_bad
.Doing setUp
Doing test_better
.Doing setUp
Doing test_builtin
```

这段代码包含一些有趣的特性：

- **第 6 行**：使用 pytest 装饰器需要导入 `pytest` 模块。
- **第 11 行**：这是一个 fixture 装饰器。其作用域为 `class`，因此对于该类，它只会在任何测试运行之前被调用一次。这一点在输出中有所体现。
- **第 16 行**：该 fixture 装饰器的作用域是 `function`，并将此函数标记为应在每个测试函数之前运行的函数。
- **第 22 行**：这是 `tearDown` 函数的定义。
- **第 25 行**：这里将 `tearDown` 函数作为终结器（finalizer）添加到请求对象中。这会导致它在测试运行后被调用。

Fixture 的灵活性远不止本例所示。尤其是，可以将各种 fixture 列为参数，仅提供给那些确实需要它们运行的函数。可以定义多个 fixture，而每个测试用例将根据其需求使用不同的子集。

解决方案是对 `os.getcwd` 进行 monkeypatch。monkeypatch 可以设置或删除以下内容：

- 属性
- 字典项
- 环境变量
- `sys.path` 中的项

monkeypatch 的使用方式类似于装饰器对象，只是 monkeypatch 对象没有被显式声明，只调用它的函数。一旦使用 monkeypatch 的函数结束，之前获取的设置会被恢复。考虑以下测试（伪）代码：

```python
def test_func1():
    logdir = os.getcwd() + "/func1/loadtests"
    logfile = open(logdir + "/results1.log", "w")
    <do test things>
    logfile.close()
```

可以通过如下方式对其进行 monkeypatch：

```python
@pytest.fixture(scope="function")
def change_cwd(monkeypatch):
    monkeypatch.setattr("os.getcwd", lambda: "/")

def test_func1(change_cwd):
    logdir = os.getcwd() + "/func1/loadtests"
    logfile = open(logdir + "/results1.log", "w")
    <do test things>
    logfile.close()
```

`change_cwd` 函数会告诉 monkeypatch 将 `os.getcwd` 函数改为其第二个参数——那个 lambda 表达式。将 `change_cwd` 作为参数传入 `test_func1`，可确保 monkeypatch 的调用被执行。其他不需要对 `os.getcwd` 做此修改的测试函数，只需不使用 `change_cwd`，就会看到正常的功能。

monkeypatch 与模拟对象（mock object）的概念类似。模拟对象是用户定义的对象，它对外暴露的接口与另一个对象相同，但实现要简单得多。例如，在测试一个从生产数据库读取数据的函数时，通常不希望使用真实的线上数据。相反，每次运行都应使用一组稳定的测试数据，以确保各次运行之间可以进行对比。在这种情况下，可以构造一个模拟数据库对象。它支持（一部分）数据库操作，但返回的是测试数据，而不是连接数据库服务器获取实时数据。monkeypatch 也执行类似的功能：它用一个简化的实现替换一个功能完整的值/函数/对象，以便进行测试。
