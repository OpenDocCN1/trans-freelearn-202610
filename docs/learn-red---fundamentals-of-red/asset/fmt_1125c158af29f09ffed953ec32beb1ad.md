
Then, we guided you through a tour of the faces available, such as `text`, `field`, `button`, `check`, `drop-list`, `panel`, `tab-panel`, `slider`, `menus`, and so on. To really build dynamic screens, you have to use `actors` on these different faces. You learned how to code them, combine multiple actors on one face, detect keys, and make timed events. We also gave an example of how you can start building more complicated maintenance apps. Finally, you got an introduction to the very powerful `draw` dialect, which enables you to draw 2D shapes and animations.

## Questions
1. Rewrite `download-rates` in `currency-rates-gui.red` so that it accepts the code and date as parameters.
2. Print out the internal structure of a `button` object to the `Red` console. Notice that `button` has a `window` parent face.
3. You can code a button to close its window by using the `quit` or the `unview` word in its `action` `facet`. Try both out. Note that `unview/all` is used when there are multiple windows to close.
4. Create a window that displays square `base` faces with different colors, such as `base yellow 25x25`. Try out several of the layout commands.
5. Make a window with a button and a checkbox. Change the state and text of the checkbox with a click on the button. After three clicks, disable that button (hint: use `face/enabled?` for that).
6. Make a window that displays the data values of a series sorted in a `text-list`. You can do this in two ways—with a button `action`, or with a `do` block.
7. Extract the names from the series `\["Dave" 1804 "Peter" 9439 "Viviane" 2386 "Marcus" 9423``\]` and show them in a `text-list`. When a name is selected, show the corresponding number in a `text` field. Also, print out the index of that name in the original list to the console.
8. Create a window displaying text that changes its background color when the mouse cursor hovers over it. Also define a list of strings, for example, cities. Define an `on-wheel` actor that displays each of the cities in turn.
9. On a rectangular `base` of width 500 and height 300 with white background, draw red diagonals in the rectangle and blue perpendicular lines.

## Further reading
You can find some additional information about `VID` here: [http://www.red-by-example.org/vid.html](http://www.red-by-example.org/vid.html).
Much more info about the drawing dialect can be found here: [http://www.red-by-example.org/draw.html](http://www.red-by-example.org/draw.html).
Official docs about the GUI system are, for the time being, rather formal and can be found here: [https://doc.red-lang.org/en/gui.html](https://doc.red-lang.org/en/gui.html). It includes in-depth info about the `View` engine, `VID`, and the `draw` dialect.

# Advanced Red
In this final chapter, we will explore some more advanced aspects of the language, and will look at where Red is heading in the near future. Reactive programming is a great way to write more responsive (GUI) apps. We encountered `Red/System` as the lower-level foundation of Red in [Chapter 1](#b2834a1f-ebba-49f2-9d61-4bdfdf8a5702.xhtml), *Red's Mission*, and [Chapter 2](#e7b0526e-a6dd-4c51-abc8-81b1dfdc8460.xhtml), *Setting Up for Development*. Here, we'll see how you can use it, either in Red apps, or standalone.

The following topics will be covered in this chapter:
* Reactive programming
* `Red/System`
* Interacting with the operating system
* Datatypes for performance
* Embedded Red and macros
* Red for smart contract programming
* Red's roadmap
* Some useful references

## Technical requirements
You'll find the code for this chapter at [https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter10](https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter10). If you have installed Red as indicated in [Chapter 2](#e7b0526e-a6dd-4c51-abc8-81b1dfdc8460.xhtml), *Setting Up for Development*, you are good to go. You can work on Windows, OS X, or Linux. You can type or paste any code in the `Red` console to see its results, or you can start using an editor such as Visual Studio Code.

## Reactive programming
In [Chapter 9](#97dee83c-19eb-4b99-8e08-720cb23a61ad.xhtml), *Composing Visual Interfaces,* we saw how easy it is to handle events on widgets by attaching actor code blocks. We encountered several examples where a change or action in one face influenced another face—for example, clicking a button changes a text in a field. The text field reacted to the button because we explicitly coded an actor on the button.

Look at the following code snippet:
```
;-- see Chapter10/reactive1.red:
point: [x: 3 y: 5]
distance: square-root (point/x ** 2) + (point/y ** 2)
print distance    ;== 5.8309518948453
point/x: 2
print distance    ;== 5.8309518948453    <-- doesn't change!
distance: square-root (point/x ** 2) + (point/y ** 2)
print distance    ;== 5.385164807134504  <-- changed
```

The `distance` word gives the distance of `point` to the origin, but when the point's coordinates change, `distance` does not change automatically with it. Only when `distance` is asked to calculate itself again after the change do we get an updated result.

You can imagine that it would be useful to have words update their values automatically when a change occurs—for example, when keeping track of a running total, or a calculation in a spreadsheet cell. Red has built this into its reactive framework. Here's how you can use it to make `distance` follow changes in `point`:
```
point: make reactor! [x: 3 y: 5]
distance: is [square-root (point/x ** 2) + (point/y ** 2)]
print distance    ;== 5.8309518948453
point/x: 2
print distance    ;== 5.385164807134504
```

The `point` word is a so-called `reactor`, the cause or `source` of a change; this is indicated with `make reactor!`. Changes in the fields of the `reactor` automatically (that is, without calling a function) trigger changes in other `target` objects, such as `distance` in this example. These target(s) are defined as a `reactive expression` by using the `is` word. Changes are propagated from `source` to `target`:
`reactor` (`source`) ----> `target(s)`.

A reactor can even update itself, which takes less code, as we can see here:
```
point: make reactor! [
    x: 3 
    y: 5 
    distance: is [square-root (x ** 2) + (y ** 2)]
]
point ;== make object! [x: 3 y: 5 distance: 5.8309518948453]
point/x: 2
point ;== make object! [x: 2 y: 5 distance: 5.385164807134504]
```

Don't let the change cause a change in itself, provoking an endless loop!

The source of the change must be an object with fields, such as `point/x` and `point/y` in the example. If the target refers to more than one nested level (such as `shape/point/x`), use `make deep-reactor!` instead of `make reactor!`.

This reactivity can also be used when writing graphical interfaces—instead of coding an actor, use the `react` word on the target side. The following example compares the synchronization between a slider and a progress bar done in two ways. In `Tab 1`, reactive programming is used (the progress bar is the target), and, in `Tab 2`, we take the actor code from the previous chapter:
```
;-- see Chapter10/reactive-gui.red
view [
    origin 0x0 space 0x0
    tab-panel 500x500 [
        "Tab 1 " [
            progress 100x20 data 20% react [face/data: s/data]
            s: slider 100x20 data 20%
        ]
        "Tab 2 " [
            p: progress 100x20 20%
            slider 100x20 data 20% [p/data: face/data]
        ]
    ]
]
```

The reactive framework is only some 250 lines of Red code and is quite versatile. An object can be the target of many source objects, and you can create chains of reacting objects as well. This framework doesn't let you do things you couldn't do before, but it makes you think in another way about problems, and your code will be more compact, readable, and declarative.
