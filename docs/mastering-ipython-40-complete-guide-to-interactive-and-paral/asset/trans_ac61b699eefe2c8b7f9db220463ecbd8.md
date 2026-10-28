

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

