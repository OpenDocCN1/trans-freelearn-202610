

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
