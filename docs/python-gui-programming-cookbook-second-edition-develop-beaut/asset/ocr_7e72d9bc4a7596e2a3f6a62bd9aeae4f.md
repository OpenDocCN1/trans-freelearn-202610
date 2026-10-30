Without using the global qualifier (line 214), we get an error.

```
213 def usingGlobal():
214 #     global GLOBAL_CONST
215     print(GLOBAL_CONST)
216     GLOBAL_CONST = 777
217     print(GLOBAL_CONST)
218 
219 
220 # call function
221 usingGlobal()
222 
<terminated> C:\Eclipse_NEON_workspace\2nd Edition Python GUI Programming Cookbook\Ch04_Code\
Traceback (most recent call last):
  File "C:\Eclipse NEON workspace\2nd Edition Python GUI Programming Cookbook\Ch
    usingGlobal()
  File "C:\Eclipse NEON workspace\2nd Edition Python GUI Programming Cookbook\Ch
    print(GLOBAL_CONST)
UnboundLocalError: local variable 'GLOBAL_CONST' referenced before assignment
```

When we qualify our local variable with the global keyword, we can print out the value of the global variable as well as overwrite this value locally:

```
213 def usingGlobal():
214     global GLOBAL_CONST
215     print(GLOBAL_CONST)
216     GLOBAL_CONST = 777
217     print(GLOBAL_CONST)
218 
219 
220 # call function
221 usingGlobal()
222 
C:\Eclipse_NEON_workspace\2nd Edition Python
42
777
```