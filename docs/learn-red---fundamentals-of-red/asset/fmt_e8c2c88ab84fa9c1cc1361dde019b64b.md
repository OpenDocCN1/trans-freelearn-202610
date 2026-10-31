
The VID dialect contains a lot of words, and we'll cover them systematically so that you don't get bewildered. In this section, we'll talk about the basic window, container, and layout settings. But to get you started with some fundamentals, we'll build a screen interface for the *Downloading currency and exchange rates* script that we made in [Chapter 7](#0f4412a2-a1fb-4241-a1cf-98fed354b645.xhtml), *Working with Files*.

# First example – a screen for downloading exchange rates

Transforming the `currency-rates2.red` script from [Chapter 7](#0f4412a2-a1fb-4241-a1cf-98fed354b645.xhtml), *Working with Files*, into a screen app is quite easy. We draw the widgets (the `text`, `input fields`, and a `button`) and provide code to execute when the button is pressed. We'll present you with the complete script called `currency-rates-gui.red`, and then explain how it works in detail.

## Running the example

Follow these steps to run the program:
* First run the app to see how it works—from the Red console, go to the folder where the script is, for example, `cd %/E/Red/code/ch9`, then type in the following—`do %currency-rates-gui.red`
* Or, first compile the script with, `red -r currency-rates-gui.red`, and then run it with, `./currency-rates-gui`

(Double-clicking the EXE in Windows will show the app window on a black console background. To avoid this, compile with `red -r -t Windows` or `red -c -t Windows`.)

The following screen appears:

![](assets/44b5bf6f-fd57-406e-83f4-e9c9e77c196e.png)

Fill in, for example, USD and the exchange rates, date (click with the mouse cursor to go the next field), click the Download XML rates file button, and after a few seconds the text SUCCESS! appears, and the XML file is downloaded:

![](assets/88671fa0-6201-4d1b-83dc-3844ca69040a.png)*TAB* key functionality for jumping from one field to another is not yet implemented in Windows, so the user must use the mouse for now. This will be solved in the forthcoming release, 0.6.4.

## Examining the code

Here is the complete script, which we'll now discuss:

```
;-- see Chapter09/currency-rates-gui.red:
Red [
    title: "Downloading currency exchange rates"
    needs: 'view                          (1)
]

url-base: http://www.floatrates.com/historical-exchange-rates.html?
download-rates: does [                    (2)
    url: rejoin [url-base "&currency_date=" curr-date/text "&base_currency_code=" curr-code/text "&format_type=xml"] 
    rates: read url
    write %rates-hist.xml rates
    out/text: "SUCCESS!"                  (3)
]

view [                                    (4)
    title "Currency rates"                (5)
    below                                 (6)
    text "Rates for which currency (format like USD):"   (7)
    curr-code: field                      (8)
    text "Rates from which date (YYYY-MM-DD):" 
    curr-date: field 100x25               (9)
    button "Download XML rates file" [download-rates]    (10) 
    out: text ""                          (11)
]
```

In line `(1)`, we see a piece of code that must always be present in the Red header of a View app, `needs: 'view`, which tells the interpreter to load the View engine, or the compiler to include it during compilation (the `'` is optional). Inside the Red GUI console, View is already present, so you can paste a `view` block in it and the window will be shown.

In line `(5)`, we specify the window's `title`. In line `(6)`, `below` specifies that all widgets are to be placed below one another. In line `(7)`, the first `text` that has to appear on the screen is declared as a string.

In line `(8)`, `curr-code: field`, an input `field`, is defined with the name `curr-code`. The date field in line `(9)` has a size `100x25` specified. On line `(10)`, a `button` widget is declared. Notice that the text on widgets such as `text` or `button` is simply the string that follows it. The button also has a `\[download-rates\]` block, which is a code section that is executed when the button is clicked (its default event). Finally, in line `(11)`, an output field is declared as empty and given the name `out`.

The `download-rates` is a simple `does` function defined in line `(2)`. It contains our previous download code, but notice how the content of the input fields is retrieved with `/text`—`curr-code/text` and `curr-date/text`. The output field's content is set in line `(3)` with `out/text: "SUCCESS!"`.

It is that easy to compose a screen and attach event code to it in Red!

\=> Now solve question 1 from the *Questions* section.

## Building a layout

The UI code starts from line `(4)`, `view \[ ... \]`. The `view` is a function that expects a window description as an argument. Note that this window description is just a code block using VID words. You could give it a name such as `win`, build this block with `append` or other code, and when you're ready, do `view win`. The `view \[\]` displays a minimal GUI window:

```
win: []
; build up win with statements like append win [title "Currency rates"]
view win
```

This allows for dynamically building screens based upon user input or events. There is also a built-in `layout` word in VID. Give your `layout` a name, and define it inside the block that follows, like this:

```
lay1: layout [a: area cyan 200x200]
```

`layout` builds a tree of `face!` objects from the block that specifies them. Then, you display it with `view`, which works on these object trees, `view lay1`. In fact, `view \[...\]` is just an abbreviation for `view layout \[...\]`.

In most of our examples, we'll stick to the static way of building our screens, but for defining menus, we'll use this way of working.

## Faces and actions

The widgets such as `text`, `field`, and `button` are called *faces* in Red. The details of a face, such as its size, color, or text, are called its *facets*. For example, in `field 100x25` the pair value `100x25` is a facet describing the size (width x height) of the field (Red knows that because a pair value with a field is always a size). The order in which the facets are written does not matter. Faces can be given a name so that their properties can be used or manipulated in code, as we saw with the `curr-code`, `curr-date`, and `out` faces.

What is displayed by `view` is in fact also a face type, namely, the `window`. A program can contain multiple views, which by default appear as successive windows when the previous window is closed.

Faces are placed inside a *container (*in this case, the window), and they are placed following a certain *layout.* In our example:
* `title "Currency rates"` is a *container* command describing the container window
* `below` is a *layout* command dictating that all of the following faces will appear below each other

The block of commands that is optionally attached to a face is called an *action facet*, or in short, an *actor.* This is code that will be executed when a certain *event* takes place, for example, clicking a button initiates the download of the XML file. An event is given the prefix `on`, such as `on-click`. Most faces respond to a default event, such as `on-click` for a `button`. For default events, you do not need to specify their name, but if you want other events, you'll have to indicate these.

A face can be `visible` or not, and when it is, it can be `enabled` or disabled. You can get that status with the functions `face/visible?` and `face/enabled?` (see question 4 in the *Questions* section).

Faces are implemented as *objects*. To see their complexity, print their contents to the console. For example, when `bt` is the name of a `button`, use `print bt` or `probe bt` in its action facet (see question 2 in the *Questions* section).

## Two-way binding
