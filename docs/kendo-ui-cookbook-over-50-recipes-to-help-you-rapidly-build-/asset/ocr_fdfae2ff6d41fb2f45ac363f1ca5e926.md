# # Creating a SplitView widget to display multiple panes in a tablet and building interaction between the panes

The Kendo UI library provides a SplitView widget, which is a tablet specific widget. A SplitView widget consists of two or more panes, and each of these panes consist of one or more view widgets. Generally, the SplitView widget is used to select an item in the list in one pane and update the other pane that displays more data about the selected item.

# # How to do it...

Let's create two panes, Left Pane and Right Pane, and then include these panes inside a SplitView widget. Here's the markup for the Left Pane:

```
```html
<div data-role="pane" id="nav-pane" data-layout="left-navbar-layout"> <div data-role="view" id="test"> <ul id="moviesList" data-role="listview" data-template="navbar-template"> </ul> </div> <script type="text/x-kendo-template" id="navbar-template"> <a href="\#movieDescription?movieID=#= data.id #" data-target="content-pane"> #= data.name # </a> </script> <div data-role="layout"
```
```