
It is easy to invoke from the command line:

```
(my_rpy2_zone)-bash-4.1$ py.test
======================================= test session starts ===
========================================
platform linux -- Python 3.4.4, pytest-2.8.7, py-1.4.31, pluggy-0.3.1
rootdir: .../chap08, inifile:
collected 0 items

======================================= no tests ran in 0.03 seconds ========================================
```

pytest will attempt to auto-locate tests according to the rules that will be covered in a later section. In this case, none were found.

## Back compatibility

It seems a shame to throw out all the work that was done on the tests from the previous sections. pytest will look for tests in files of the form test_*.py (and *_test.py). The laziest thing possible would be to just copy test1.py into test_1.py and see what happens:

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

- `recurse` into directories, unless they match `norecursedirs`.
- `test_*.py` or `*_test.py` files, imported by their test package name.
- Test prefixed test classes (without an `__init__` method).
- `test_` prefixed test functions or methods are test items.

## Organizing test files

pytest allows test functions to be included in the same file as the functions to be tested. This has some advantages in that it makes it more likely that tests will be updated when functionality is, and makes the tests easy to find.

However, this is not a good idea for any but the smallest project, for several reasons:

- A good test suite can be several times the size of the code to be tested. Putting both in the same place obscures the code.
- It is often the case that different groups develop tests and code. Using different files makes for easier version control.
- Tests should not depend on how code is written, nor should code depend on how tests are structured. The easiest way to ensure this is to keep them completely separate.

When tests are no longer in the same file as code, it is important to keep them somewhere easy to find and associate with the code they test. A parallel directory structure is an easy way to achieve this:

```
pkg/
    __init__.py
    prog1.py
    …

tests/
    test_prog1.py
    …
```

Note the lack of an `__init__.py` file in the tests directory. This provides an easier time for one of the test discovery rules listed previously. There is no requirement in pytest that things be mirrored this exactly; it is just a useful heuristic.

## Assertions

pytest uses the assert statement found in Python. This obviates the need to remember all the different `assert*` methods from unittest. Consider the difference in readability between this:

```
self.assertEqual(f(i), ans[i])
```

And this line of code:

```
assert f(i) == ans[i]
```

In general, pytest tests are smaller in terms of lines of code. The `test2.py` test file can be rewritten under pytest as follows:

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

Here is the result:

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

Even the results can be abbreviated with the `-q` command-line argument:

```
In [3]: !py.test -q test_2.py
.
1 passed in 0.01 seconds
```

## A test using setUp and tearDown

unittest depends on overloading the setUp and tearDown functions from its parent class. As pytest drops the parent class, it needs a different mechanism. In fact, it has two: a classic xUnit-style and one using fixtures.

### Classic xUnit-style

The following methods are provided to do setup and teardown:

| Method | Effect |
|---|---|
| setup_method(self, method) | Called before every test method in a class |
| teardown_method(self, method) | Called after every test method in a class |
| setup_function(function) | Called before every test method in a module |
| teardown_function(function) | Called after every test method in a module |

This makes rewriting our first random number test a breeze:

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

## But It Worked in the Demo! – Testing

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

## Using fixtures

A function can be decorated as a pytest fixture. This fixture decoration can take a "scope" argument that specifies when the fixture should be called:

| Scope | Effect |
|---|---|
| session | The function is executed once, at the beginning of the session |
| module | The function is executed once, at the beginning of the module |
| class | The function is executed once, at the beginning of the class |
| function | The function is executed before every test case |

Our second random number testing class can then be rewritten as follows:

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
