```
```python
self.context = glcanvas.GLContext(self)

def OnPaint(self, event):
    dc = wx.PaintDC(self)
    # self.SetCurrent()          # commented out because:
    self.SetCurrent(self.context)  # We have to pass in a context
```
```

We now can create the following GUI:

```
```python
import_OpenGL_cube_and_cone.py
```
```

![](bbox=[0.113, 0.313, 0.972, 0.723])

In the classic version of wxPython, SetCurrent() did not require a context. Here is some code we might find when searching online:

```
```python
def OnPaint(self, event):
    dc = wx.PaintDC(self)
    self.SetCurrent()
    if not self.init:
        self.InitGL()
        self.init = True
    self.OnDraw()
```
```