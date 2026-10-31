
User interface screens and all they contain are updated automatically in real time when their properties change. A *two-way binding* exists between faces and facets. When either one of them changes, the other follows accordingly. We won't use it in our examples, but if you as a developer want to control when the screen is updated, you need to do the following:

*   Turn the `auto-sync` option off—`system/view/auto-sync?: off`
*   Use the `show` function such as `show face` to update the widget

\=> Find out in question 3 from the *Questions* section how to code a button to close the window.

# Container settings – view

These are settings that describe the look and feel of the container facet (the window). We already used `title`. Here are some common settings that can be combined in any order (paste the code in the Red console, and play with it):

*   `view [ size 350x125]` allows windows with different sizes (width x height)
*   `view [ backdrop blue]` creates a window with a blue background

# Changing an app's icon

The default Red window *icon* (displaying the towers of Hanoi) can be replaced with your own `.ico` file as follows:

```
;-- see Chapter09/icon.red:
Red [
    needs: 'view
    icon: %gaming.ico
]
view []
```

This window displays as:

![](assets/b2a0b307-c367-46f4-b945-6bcf9bebc576.png)

But to see this new icon, the app must be compiled as `red -r icon.red`. This also works for the icon of a non-GUI app shown in the app-tray.

# View's refinements

`view` can also have one or several refinements. Their details are described in blocks that come after the main `view` block, as you can see in the following schema of a general `view` block:

```
view/refine1/refine2 [
    container settings
    layout commands
    face1 facets [action facet]
    face2 facets [action facet]
    ...
][details refine1][details refine2]
```

To determine where a window must appear on the screen, use the `options` refinement with a block giving the `offset` value, like this:

```
;-- see Chapter09/container-layout.red:
view/options [
][offset: 400x100]
```

You might have noticed that a standard window is not resizable or maximizable. To accomplish this, we need the `flags` refinement with the `resize` word:

```
view/flags [
][resize]
```

These flags can be combined as in this example, where the window is resizable and modal:

```
view/flags/options [
][modal resize][offset: 400x100]
```

Modal means that the window is displayed on top of the parent window, which is disabled as long as the modal window is shown. In other words, a modal window demands your attention.

There are other flags such as `no-title`, `no-border`, `no-min`, `no-max`, and `no-buttons`. Their names give away what they do. Combine them to make minimal windows when appropriate.

Here is an example of the use of these flags:

```
;-- see Chapter09/digital-clock.red:
time-now: to string! now/time
view/flags [ 
    title "Red Time"
    show-time: text font-size 14 font-color red time-now
]
[modal popup]
```

This displays the following window:

![](assets/e79da340-8a86-40bd-902f-effa00bcb241.png)

It also shows the use of `font-size` and `font-color` for `text`. To make the clock run, we need only one line of code, but we'll need an event for that, see the *Events and actors*, and *Timer events* sections later.

# Using do in view

Inside the `view` code block, ordinary binding statements such as `n: 2 ** 3` and `print n` *must* be placed within a `do` block, like this:

```
;-- see Chapter09/container-layout.red:
view [
    text "Use of do block"
    do [n: 2 ** 3 print n]
]
```

A `do` block can be used to evaluate code and initialize or make changes to the GUI, before the layout is displayed.

All other code must appear within action facet code blocks, such as our button-click code `[download-rates]`.

# Layout commands

Red works with the normal *coordinate system*, with the origin `0x0` in the top-left corner:

![](assets/8eb09781-0076-48a6-ba5c-e5b8917de5ea.png)

A point with coordinates (`n`, `m`) is noted as `nxm`. Such a `pair!` value can also be used in the `face/offset` property, which describes the position of the top-left corner of the face from the origin, in this case `n` pixels from the left, and `m` pixels down. It can also be used to move a face to a new position:

```
;-- see Chapter09/offset.red:
t: text "I will not stay here"
button "Move text" [t/offset: 20x30] 
```

By default, Red uses a kind of flow layout represented with the `across` word. Faces are placed from left to right until the screen line is full, then the line below is filled, and so on.

A number of words besides `across` and `below` exist to fine-tune this process:

*   `return` places the next face on a line horizontally below while in `across` mode, or on a line vertically beside it while in `below` mode
*   `at nxm` places the next face (and only that) with its top-left corner at an absolute position (`n`, `m`)
*   `origin nxm` starts the window layout at position (`n`, `m`) instead of (0, 0)
*   `pad wxh`, where `w` is width and `h` is height, specifies that the next face starts `w` pixels to the right and `h` pixels down
*   `space wxh`, where `w` is width and `h` is height, specifies the new spacing between the next faces in width and height (in other words, the offset)

To get a feeling for how these commands work, experiment with them in question 4 from the *Questions* section.

# A faces walkthrough

The VID dialect contains some 20 different faces (graphical widgets), each with its own specific properties (*facets*). The type of that value specifies what the facet does. For example, a string will be used as the displayed text for the face, or a pair will determine its size. Most faces also have a default *action facet*, that is, a block of commands that is triggered by a certain event, such as clicking on a button or selecting from a list.

We cannot possibly discuss them all in detail, instead we'll do a walkthrough of the kinds of faces that are available, what you can do with them, and highlight some important properties. You're encouraged to try these out (together with the questions), as we go along.

# Base, box, and image

We start with the most basic types:

![](assets/06c162b0-70c9-4b36-9d06-ef0109868d77.png)

Here is the VID code:

```
;-- see Chapter09/base-box-image.red:
view [
    base
    box red
    image 140x140 %heads.jpg
    below
    text "I am a button:"
    button %heads.jpg [quit]
]
```

`Base` is the most primitive of them all. It displays a gray rectangle, but it can respond to a click (`on-down`) event; it can show an image, and it can be used to draw inside it. A `box` is like a base, but it is transparent by default, so we give it a color to make it show.

An `image` is a base face with a default size of 100 x 100, used to show a picture. Whenever a face has an image file as a facet, it is used as a background image for that face. See the button example. A `camera` facet is also available to capture a video stream when that hardware is available, for example on mobile devices.

# Text faces – h1, text, field, and area

Here is a window using text faces:

![](assets/a9ff03ce-e707-4dd0-b476-b969776615f9.png)

Here is the code to produce that window (note that in a real app we must give our widgets meaningful names, instead of `t1` and `f1`!):

```
; see Chapter09/text-faces.red:
view [
    title "Text faces"
    below
    h3 "Heading 3"
    t1: text 300x30 bold italic blue font-name "algerian" 
        font-color ivory font-size 14 "Text with properties"
    text "Click me!" [face/color: orange face/text: "FIRE!"]
    f1: field 300 "Type something here" [ print [f1/data] ]
    a: area 300x100 [print a/text]
]
