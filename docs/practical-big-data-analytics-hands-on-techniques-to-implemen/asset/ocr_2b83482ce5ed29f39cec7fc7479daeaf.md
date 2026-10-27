# Big Data With Hadoop

```
```python
import sys
sys.path.append('.')

for line in sys.stdin:
    line = line.strip()
    keys = line.split()
    for key in keys:
        value = 1
        print("%s\t%d" % (key,value))
```
```

```
```shell
[cloudera@quickstart cyrus]$ vi reducer.py # Copy-Paste the content
of reducer.py as shown below using the vi or nano Unix editor.

[cloudera@quickstart cyrus]$ cat reducer.py
```
```

```
```python
#!/usr/bin/env python

import sys
sys.path.append('.')

last_key = None
running_total = 0

for input_line in sys.stdin:
    input_line = input_line.strip()
    this_key, value = input_line.split("\t", 1)
    value = int(value)

    if last_key == this_key:
        running_total += value
    else:
        if last_key:
            print("%s\t%d" % (last_key, running_total))
        running_total = value
        last_key = this_key

if last_key == this_key:
    print("%s\t%d" % (last_key, running_total))
```
```

```
```shell
[cloudera@quickstart cyrus]$ chmod 755 *.py
```
```