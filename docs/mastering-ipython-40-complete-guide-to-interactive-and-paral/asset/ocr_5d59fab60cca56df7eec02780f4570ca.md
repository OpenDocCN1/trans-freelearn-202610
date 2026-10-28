```
self.vals[tmp] = self.vals[tmp] + 1

chi2, p = scipy.stats.chisquare(self.vals)
assert p > 0.05

testrand2.py
```

With appropriate editing (the addfinalizer line must be removed — in my environment, it produced a Python INTERNALERROR), it runs and produces the following output:

```
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

This code contains some interesting features:

+   - **Line 6**: The use of the pytest decorators requires importing the `pytest` module.
+   - **Line 11**: This is a fixture decorator. The scope is `class`, so it will be called only once for this class, before any of the tests are run. This is reflected in the output.
+   - **Line 16**: This fixture decorator has the `function` scope, and marks this function as one that should be run before every test function.
+   - **Line 22**: This is the definition of the `tearDown` function.
+   - **Line 25**: Here, the `tearDown` function is added to the request object as its finalizer. This will cause it to be called after the test has run.

Fixtures are even more flexible than this example can show. In particular, the various fixtures can be listed as parameters to only those functions that actually need them to run. It would be possible to define multiple fixtures, and each test case would use a different subset based on its requirements.