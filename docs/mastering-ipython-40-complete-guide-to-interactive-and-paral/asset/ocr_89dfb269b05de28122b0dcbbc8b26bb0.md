```
244      self.result = testRunner.run(self.test)
245      if self.exit:
--> 246      sys.exit(not self.result.wasSuccessful())
247
248 main = TestProgram
```

SystemExit: True

In this case, it's not that interesting, but the information is there if needed.

A small fix should remedy this problem:

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

Along with a corresponding change to the test (normally, one would just change the original source, but this is clearer for instructional purposes):

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