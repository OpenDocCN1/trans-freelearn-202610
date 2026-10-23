# # Pylogix

Pylogix (https://github.com/dmroeder/pylogix) is a Python module aimed at allowing Python scripts to interact with an Allen Bradley ControlLogix PLC. It makes interactions very straightforward and can be used to find out PLC details, find configured tags, and to read and write to any tag. You can create scripts to do all this or use an interactive Python shell to run commands one at a time – well… interactively.

To install Pylogix and IPython, run the following command on the Kali Linux machine:

```
```
pac@KVM0101011:-$ pip3 install --upgrade pylogix ipython
Defaulting to user installation because normal site-packages is not writeable
Collecting pylogix
  Downloading pylogix-0.7.0-py2.py3-none-any.whl (61 kB)
...
Installing collected packages: pylogix
Successfully installed pylogix-0.7.0
```
```

Now that Pylogix and IPython have been installed, we will use the two to connect to the Allen Bradley ControlLogix PLC at 172.25.100.11, read all the configured tags for the running program, select an interesting tag, and read and write values to that tag:

```
```
pac@KVM0101011:-$ ipython
Python 3.8.5 (default, Aug  2 2020, 15:09:07)
Type 'copyright', 'credits' or 'license' for more information
IPython 7.18.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from pylogix import PLC

In [2]: comm = PLC()

In [3]: comm.IPAddress = '172.25.100.11'

In [4]: comm.GetTagList()
Out[4]: Response(TagName=None,
Value=[LgxTag(TagName=Program:MainProgram, InstanceID=7162,
SymbolType=104, DataTypeValue=104, DataType=, Array=0,
Struct=0, Size=0 AccessRight=None Internal=None Meta=None
Scope0=None Scope1=None Bytes=None), LgxTag(TagName=Boiler2_
```
```