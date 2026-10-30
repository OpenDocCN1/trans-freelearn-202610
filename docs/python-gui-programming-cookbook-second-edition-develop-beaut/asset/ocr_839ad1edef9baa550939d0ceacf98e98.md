The online documentation is located at the https://pyglet.readthedocs.org/en/pyglet-1.2-maintenance/ website for the current release:

![](bbox=[0.115, 0.165, 0.975, 0.445])

A first experience using the pyglet library may look as follows:

```
import pyglet
window = pyglet.window.Window()
label = pyglet.text.Label('PyGLet GUI',
    font_size=42,
    x=window.width//2, y=window.height//2,
    anchor_x='center', anchor_y='center')

@window.event
def on_draw():
    window.clear()
    label.draw()

pyglet.app.run()
```

The preceding code is from the official pyglet.org website and results in the following fully functional GUI:

pyglet_GUI_Simple.py