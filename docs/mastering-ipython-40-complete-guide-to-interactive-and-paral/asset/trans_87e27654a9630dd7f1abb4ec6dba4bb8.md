

- 块注释应与它们所适用的代码保持相同的缩进级别。每一行应以 # 符号和一个空格开头。
- 注释很少应与代码写在同一行。

## 函数注解

函数注解和类型提示可以被视为一种内联注释的类型。函数注解起源于 PEP 3107，该 PEP 提供了为 Python 函数添加任意元数据注解的语法。从定义（https://www.python.org/dev/peps/pep-3107/）中：

> "由于 Python 的 2.x 系列缺乏标注函数参数和返回值标准方式的手段，出现了各种工具和库来填补这一空白。(...) 本 PEP 旨在提供一种单一、标准的方式来指定此信息，减少到目前为止因机制和语法差异过大而造成的混乱。"

### 语法

函数注解适用于两类事物：参数和返回值。

### 参数

对于参数，注解由冒号表示，其方式类似于用等号标记默认值，如此示例所示：

```python
def foo(x: "int >= 0"):
```

注解和默认值都可以按顺序放置来同时指示，如下所示：

```python
def foo(x: "int >= 0" = 0):
```

非正式地，参数可以这样描述：

```text
identifier [: expression] [= expression]
```

这里，注解和默认值都是可选的。

### 返回值

函数返回值可以通过在参数列表后跟 -> 和一个表达式来注解。以下是示例：

```python
def foo(x: "int >= 0" = 0) -> "seemingly random integer":
    return 42
```

### 语法

类型提示使用相同的语法并适用于与函数注解相同的事物。主要扩展是它们允许注解为一个类。完整规则集很复杂，但以下提示应涵盖最常见的情况：

- 内置类
- 用户定义类
- `types` 模块中的类型
- 抽象基类
- 特殊构造，包括 `None`、`Any`、`Union`、`Tuple` 和 `Callable`

PEP 的很大一部分致力于特殊情况和复杂类型，包括泛型、协变和逆变、前向引用以及存根文件。

### 语义

与 PEP 3107 一样，类型提示没有关联的标准语义。其目标是提供第三方工具可以使用的标准。在类型提示具有语义的程度上，其语义是由使用它们的工具提供的，而不是由 Python 语言本身提供的。从定义中：

> "本 PEP 旨在提供类型注解的标准语法，使 Python 代码更容易进行静态分析和重构，可能的运行时类型检查，以及（也许在某些上下文中）利用类型信息进行代码生成。"

形成该标准的重要工具是 mypy（http://mypy-lang.org/）。mypy 的目的是为 Python 提供静态类型检查器。虽然在文档和调试中静态类型检查的优点可以争论，但拥有该选项是很好的。

## 文档字符串

Python 文档字符串是一个字符串字面量。它可以作为模块中的第一个语句出现，也可以作为函数、方法或类定义后的第一个语句出现。文档字符串成为该对象的 __doc__ 特殊属性。其目的是提供一种更自由的方式来记录程序的一部分。

### 示例

以下代码为一个简化的早期测试套件提供了文档字符串：

```python
"""
This is an abbreviated version of my random number generator test
suite.

It uses the pytest framework.  It does not do much in this form.
"""

import numpy as np
import scipy.stats
import random

class TestRandoms( ):
    """
    This is the main class.

    Normally it would hold all the tests, plus and setup and
    tearDown fixtures.
    """
    def test_builtin(self):
        """
        Test the built-in random number generator on 10000
        numbers.
        """
        num_tests = 10000
        vals = [0 for i in range(10)]
        for i in range(num_tests):
            tmp = random.randint(0, 9)
            vals[tmp] = vals[tmp] + 1

        chi2, p = scipy.stats.chisquare(self.vals)
        assert p > 0.05

def foo( ):
    """ I just needed a function outside of a class as an
example"""
    pass
```

code1.py

### 文档

文档字符串可以通过使用 Python 内置的 help 函数来访问。结果以标准帮助方式显示（即如同用 less 查看文本文件），如下所示：

```text
In [4]: import code1

In [5]: help(code1)
Help on module code1:

NAME
    code1 - This is an abbreviated version of my random number generator test suite.

DESCRIPTION
    It uses the pytest framework.  It does not do much in this form.

CLASSES
    builtins.object
        TestRandoms

    class TestRandoms(builtins.object)
    |   This is the main class.
    |
    |   Normally it would hold all the tests, plus and setup and teardown fixtures.
    |
    |   Methods defined here:
    |
    |   test_builtin(self)
    |       Test the built-in random number generator on 10000 numbers.
    |
    |   --------------------------------------------------------------------------
    |
    |   Data descriptors defined here:
    |
    |   __dict__
    |       dictionary for instance variables (if defined)
    |
    |   __weakref__
    |       list of weak references to the object (if defined)

FUNCTIONS
    foo()
        I just needed a function outside of a class as an example

FILE
    /nfs/02/wit0096/Packt/chap09/code1.py
```

可以通过命名它们来访问各个部分的文档字符串。

这里，我们访问类的 `help`：

```text
In [6]: help(code1.TestRandoms)
Help on class TestRandoms in module code1:

class TestRandoms(builtins.object)
 |  This is the main class.
 |
 |  Normally it would hold all the tests, plus and setup and teardown
    fixtures.
 |
 |  Methods defined here:
 |
 |  test_builtin(self)
 |      Test the built-in random number generator on 10000 numbers.
 |
 |  ----------------------------------------------------------------------
 |  Data descriptors defined here:
 |
 |  __dict__
 |      dictionary for instance variables (if defined)
 |
 |  __weakref__
 |      list of weak references to the object (if defined)
```

```python
self.assertEqual(f(i), ans[i])
```

```python
def foo():
    """
    An independent function.

    I needed another function to illustrate the docstring for a
    function that was not a member of a class.
    """
    pass
```

code2.py

它产生以下（简略）输出：

```text
In [10]: import code2

In [11]: help(code2)

Help on module code2:

NAME
    code2 - This is my hailstone unit test suite.

DESCRIPTION
    It uses the unittest framework.  Admittedly, it does not do much.

CLASSES
    unittest.case.TestCase(builtins.object)
        TestHailStones

        class TestHailStones(unittest.case.TestCase)
        |   The main class for testing the hailstone sequence generator.
        |
        |   Method resolution order:
        |       TestHailStones
        |       unittest.case.TestCase
        |       builtins.object

addTypeEqualityFunc(self, typeobj, function)

Add a type specific assertEqual style function to compare a type.

This method is for use by TestCase subclasses that need to register
their own type equality functions to provide nicer error messages.

Args:
    typeobj: The data type to call this function on when both values
        are of the same type in assertEqual().
    function: The callable taking two arguments and an optional
        msg= argument that raises self.failureException with a
        useful error message when the two arguments are not equal.

assertAlmostEqual(self, first, second, places=None, msg=None, delta=None)
```

注意 `unittest.TestCase` 类的文档无需额外努力即可包含在内。

### 推荐元素

PEP 0257 记录了与文档字符串相关的约定。从标准（https://www.python.org/dev/peps/pep-0257/）中：

> "本 PEP 的目标是标准化文档字符串的高级结构：它们应包含什么内容，以及如何表述（而不涉及文档字符串内的任何标记语法）。PEP 包含约定，而非法律或语法。"

虽然开发者没有必须遵循这些指南的要求，但第三方工具（如 Docutils 和 Sphinx）可能依赖它们从文档字符串中提取特性。

