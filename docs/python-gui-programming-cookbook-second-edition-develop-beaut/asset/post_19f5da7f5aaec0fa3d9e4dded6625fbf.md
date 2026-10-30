
Here is the code that runs when clicking on Tab 3 of the first notebook:

```
#----------------------------------------
def display_tab3():
    monty3 = ttk.LabelFrame(display_area, text=' New Features ')
    monty3.grid(column=0, row=0, padx=8, pady=4)

    # Adding more Feature Buttons
    startRow = 4
    for idx in range(24):
        if idx < 2:
            colIdx = idx
            col = colIdx
        else:
            col += 1
        if not idx % 3:
            startRow += 1
            col = 0

        b = ttk.Button(monty3, text="Feature " + str(idx + 1))
        b.grid(column=col, row=startRow)

    # Add some space around each label
    for child in monty3.winfo_children():
        child.grid_configure(padx=8)
```

Clicking any tab other than the first three tabs of notebook one calls the same function, display_button(), which results in a button being displayed whose text property is being set to show the notebook and tab number:

```
#----------------------------------------
def display_button(active_notebook, tab_no):
    btn = ttk.Button(display_area, text=active_notebook + ' - Tab ' + tab_no, \
                     command= lambda: showinfo("Tab Display", "Tab: " + tab_no ))
    btn.grid(column=0, row=0, padx=8, pady=8)
```

Clicking any of these buttons results in a Message box.

At the end of the code, we invoke the display_tab1() function. When the GUI first starts up, the widgets of this tab are what get displayed in the display area:

Clicking on **Tab 2** of the first notebook clears the tab display area and then displays the widgets created in the `display_tab2()` function:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_438_0.png)

Note how the tab display area automatically adjusts to the sizes of the widgets being created.

Clicking **Tab 3** results in the following GUI display:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_438_1.png)

Clicking any other tab in either the first or the second notebook results in a button being displayed in the tab display area:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_439_0.png)

Clicking any of those buttons results in a Message Box:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_439_1.png)

In this recipe, you learned how to create more than one notebook within the same GUI design.

There is no limit to creating notebooks. We can create as many notebooks as our design requires.

![](img/986b0dbf3f539f4f3baa82bd3003f04e_440_0.png)

In programming, at certain times, we run into a wall and get stuck. We keep banging our head against this wall but nothing happens.
Sometimes, we feel like we want to give up.
Miracles do happen...
By keeping on banging against this wall, at a certain moment in time, the wall will collapse and the road will be open.
At that point in time, we can make a positive dent in the software universe...

# Index

+   ### 3
+   
+   3D
+   
+   - custom GUI, creating with wxPython 345
+   
+   __init__
+   
+   - used, for connecting modules 383
+   
+   __main__ section
+   
+   - used, for creating self-testing code 290, 295

+   ### B
+   
+   bitmaps
+   
+   - used, for creating GUI 351

+   ### C
+   
+   Callback functions
+   
+   - writing 123, 124, 125
+   
+   canvas widget
+   
+   - using 99, 100, 101
+   
+   chart
+   
+   - creating 142, 143
+   - creating, Matplotlib used 132, 133, 134, 135
+   - labels, placing 144, 147, 148, 149
+   - legend, adding 150, 152
+   - scale, adjusting automatically 156, 157, 158, 159, 160
+   - scaling 153, 155, 156
+   
+   check button
+   
+   - creating, with different initial states 26
+   
+   classes
+   
+   - coding, to improve GUI 117, 118, 122, 123
+   
+   code naming convention
+   
+   - using 394
+   
+   combo box widgets
+   
+   - adding 24, 25, 26
+   
+   complexity

+   - avoiding 405
+   
+   Coordinated Universal Time (UTC) 267
+   Create, Read, Update, Delete (CRUD) 208
+   custom GUI
+   
+   - colors, adding 357
+   - creating, with wxPython 345

+   ### D
+   
+   daemon 173
+   
+   data
+   
+   - obtaining, from widget 111, 112
+   - reading, from websites with urlopen 201, 205
+   - retrieving, from MySQL database 240, 244
+   - storing, from MySQL database 240, 244
+   
+   debug output levels
+   
+   - configuring 286, 289
+   
+   debug watches
+   
+   - setting 281
+   
+   design patterns
+   
+   - using 402
+   
+   dialog widgets
+   
+   - used, for copying files to network 187, 197
+   
+   Don't Repeat Yourself (DRY) 30, 126

+   ### E
+   
+   Eastern Daylight Time (EDT) 268
+   Eclipse PyDev IDE
+   
+   - used, for writing unit tests 299, 305
+   
+   Eclipse, with PyDev plugin
+   
+   - reference link 285
+   
+   Entry widget 19, 20
+   exception handling
+   
+   - reference link 202

+   ### F
+   
+   fall-down

+   and OOP code, mixing 390
+   First In First Out (FIFO) 177
+   fixtures
+     about 300
+     references 300
+   Frame 318

# G

+   GLCanvas update
+     URL, for example 343
+   grid layout manager
+     using 69
+   GUI language
+     modifying 258, 261
+   GUI widgets
+     aligning, by embedded frames within frames 50, 51, 52, 53, 54, 55
+   gui2py
+     URL 351
+   GUI
+     code, testing 277, 281
+     communicating 333
+     creating, multiple notebooks used 411
+     creating, PyGL GUI development framework used 354
+     creating, to PyOpenGL 341
+     creating, unit tests used 295, 299
+     designing, in agile fashion 273, 277
+     localizing 263
+     preparing, for internationalization 269

# I

+   independent message boxes
+     creating 79, 80, 81, 82
+   Integrated Development Environments (IDEs) 281
+   Inter Process Communication (IPC) 162, 308

# L

+   Label 51
+   label frame widget
+     labels, arranging 39, 40, 41, 42
+   labels
+     adding, to GUI form 14
+     arranging, in label frame widget 39, 40, 41, 42
+     placing, on charts 144, 147, 148, 149

+   Last In First Out (LIFO) 177
+   legend
+     adding, to chart 150, 152
+   loop
+     several widgets, adding 34

# M

+   main root window
+     icon, modifying 84, 85
+   Matplotlib
+     installing, pip used with whl extension 135, 136, 137, 138, 140, 141
+     URL 132, 134
+     URL, for downloading 138
+     used, for creating charts 132, 133, 134, 135
+   menu bars
+     creating 55, 57, 58, 60, 61, 62, 63
+   message boxes
+     creating 74, 75, 76, 77, 78
+   mighty 51
+   module-level global variables
+     using 113, 115, 116, 117
+   modules
+     queues, passing 183, 186
+     multiple notebooks
+       used, for creating GUI 411
+   MySQL database
+     connection, configuring 215, 219
+     data, retrieving 240, 244
+     data, storing 240, 244
+   MySQL server
+     connecting, from Python 209
+     installing, from Python 209
+     URL, for installing 209
+   MySQL Workbench
+     about 244
+     references 246
+     using 244, 250

# O

+   Object Oriented Programming (OOP)
+     about 93
+     avoiding 398
+     code, mixing with fall-down 390
+   OpenGL

animation 361
URL 368

P
padding
  used, for adding space around widgets 42, 43, 44, 45
Phoenix version 309
pip module
  used, for installing Matplotlib with whl extension 135, 136, 137, 138, 140, 141
progressbar
  adding, to GUI 96, 97, 98, 99
PyGLet GUI development framework
  used, for creating GUI 354
Pyglet
  URL 356
PyOpenGL 3.0.2
  URL 342
PyOpenGL
  about 339
  importing 341
Python 3.6
  URL 9
Python connector driver
  reference link 212
Python Enhancement Proposal (PEP) 373
  URL 9
Python extension packages
  URL 342
Python GUI database
  designing 227
Python GUI
  buttons, creating 16, 18
  buttons, used for changing text property 16, 18
  creating 9, 10
  database, designing 220
  label, adding 14
  resize, preventing 12
Python
  MySQL server, connecting to 209
  MySQL server, installing 209
  used, for controlling different GUI frameworks 330
  used, for creating tooltips 92, 93, 94, 95, 96

Q
queues
  passing, among modules 183, 186
  using 177, 183

R
radio button widgets
  using 28, 29, 30, 31
Red, Green, Blue, Alpha (RGBA) color mode 362
regression 281
relief property
  using 89, 90, 91, 92
reusable GUI components
  creating 125, 126, 127, 128

S
SCHEMAS 249
scrolled text widgets
  using 31, 32, 33
self-testing code
  creating, __main__ section used 290, 295
slideshow
  creating, tkinter used 368
spaghetti code
  avoiding 376
spin box control
  using 85, 86, 87, 88, 89
SQL DELETE command
  using 236, 240
SQL INSERT command
  using 227, 230
SQL UPDATE command
  using 230, 235
Standard Time (EST) 268
StringVar()
  using 105, 108, 109, 110

T
tabbed widgets
  creating 63, 64, 65, 66, 67, 68
tcl
  URL 31
TCP/IP
  using, to communicate via networks 198
