# # Working with Raphaël rather than SVG directly

Browser support aside, one good reason for using Raphaël over writing raw SVG is that it makes vector drawing _easier_. Take for example, drawing a rectangle and animating its width from 50 pixels to 100 pixels using raw SVG:

```
```
<rect x="10" y="10" width="50" height="30">
  <animate attributeType="XML"
           attributeName="width"
           to="100"
           fill="freeze"
           dur="1s" />
</rect>
```
```

Here we draw a rectangle at (10, 10) with a width of 50 and a height of 30 using the `<rect>` element. The nested `<animate>` element then defines an animation over one second on the rectangle's width to a total width of 100, where `fill="freeze"` is used to retain the state of the rectangle after the animation is complete (otherwise its width would be reset to 50). This is fairly verbose given that what we are looking to achieve is quite straightforward. The equivalent animation in Raphaël is achieved by the following code:

```
```
var rect = paper.rect(10, 10, 50, 30);
rect.animate({
  width: 100
}, 1000);
```
```

Here, the `animate` and `rect` methods are more concise.

Raphaël also integrates well with other libraries, for example, jQuery, owing to being compact and appropriately namespaced (it creates just one global variable named Raphael).

# # Applications of Raphaël

Raphaël has been used in a variety of projects ranging from map visualization during the _Town Hall @ the White House_ event (`http://askobama.twitter.com/`) to online games such as **Ebocs** (`http://www.dejapong.com/ebocs/`). The former usage demonstrates a popular application of the library, namely in cartography, while the latter is a less common but a viable application of the library for creating games with a low sprite (bitmap image) count.