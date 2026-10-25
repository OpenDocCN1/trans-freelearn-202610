The result is shown as follows:

![](bbox=[0.214, 0.177, 0.785, 0.346])

# # Working with text

Drawing text in the canvas rather than as HTML markup with CSS styling allows us
to animate and transform it in the same way as we would for other shapes. Text is
created using the `text` method and its properties (such as `size` and `font-family`)
are modifiable as attributes. The `text` method has the following definition, where the
`text` parameter is our text string and accepts the standard escape sequence `\n` as
input, which places the proceeding text onto a new line:

Paper.text(x, y, text)

The following code creates two pieces of text using this method and then groups
them into a set in order to apply `font-size` and `font-family` attributes to
both elements:

```
```
var text1 = paper.text(0, 15, 'I am text anchored start.');
text1.attr({
    'text-anchor': 'start',
});
var text2 = paper.text(270, 100, 'I am text\nanchored middle');
text2.attr({
    'text-anchor': 'middle'
});
var group = paper.set(text1, text2);
group.attr({
    'font-size': 20,
    'font-family': 'serif'
});
```
```

[ 28 ]