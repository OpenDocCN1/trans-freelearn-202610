# # How to do it...

Let's first create a textarea element, specifying the number of rows, columns, and width:

```
`<textarea id="editor"
    row="20"
    cols="80"
    style="width: 700px;">
</textarea>`
```

The next step is to use this textarea element and create an Editor by invoking the
kendoEditor function:

```
$("#editor").kendoEditor();
```

The kendoEditor function is used to initialize textarea as an Editor widget. The Editor will
now have a WYSIWYG interface, allowing the user to format the text.

# # How it works...

When you initialize the textarea element as an Editor by invoking the kendoEditor
function, various tools are available in the toolset, as shown in the following screenshot:

![](bbox=[0.135, 0.511, 0.901, 0.727])

The toolset bar is shown at the top of the Editor and it allows a user to format the entered
data. This includes applying different formatting for the entered text, marking the text as bold
or italic, underlining the selected text, aligning the text, creating a bulleted or numbered list,
and adding images and tables.