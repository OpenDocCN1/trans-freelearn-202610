# Introduction

However, the main aim of this book is to teach you the underlying concepts of Facebook Flash development; once you understand these, the actual code and the SDK used don't matter. For this reason, this book will also teach you how to program every sort of Facebook interaction you might need from scratch. The code will be all yours, and you'll understand every line, with no abstraction in the way.

Besides the Adobe AS3 SDK for Facebook Platform, two other code libraries are used heavily:

+   - **MinimalComps**: Keith Peters' excellent, lightweight user interface components, available at http://www.minimalcomps.com/ under an MIT license.
+   - **as3corelib**: A collection of classes and utilities for working with AS3, including classes for JSON serialization, available at https://github.com/mikechambers/as3corelib under a BSD license.

# Debugging

From Chapter 3 onwards your SWF will need to be run from your server, through a web browser, in order to work. (Find out why in that chapter.) This makes debugging tricky—there's no Output panel in the browser, so trace statements aren't automatically visible.

The Visualizer contains a dialog feature which you can use to work around this. It can be created from any class that is in the display list. To do so, first import the DialogEvent class:

```
import events.DialogEvent;
```

Then, dispatch a DialogEvent of type DIALOG with an argument containing the text you wish to see output:

```
dispatchEvent(new DialogEvent(DialogEvent.DIALOG, "Example"));
```

It will look like this:

![](bbox=[0.283, 0.617, 0.722, 0.775])

Of course, that's useful only for the Visualizer project. What can you do when you build your own?