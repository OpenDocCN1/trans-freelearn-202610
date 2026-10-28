# # Assertions

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