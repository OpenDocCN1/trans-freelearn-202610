```
data-bind="click: updateVisible">
Toggle Visible
</button>
</div>
```

In the previous code snippet, the span element has the data-bind attribute set to visible:
isVisible. Here, the value of the isVisible model attribute should be of the Boolean type.
If the same is false, then the style attribute of the span element is set to display: none.
Also, the button has the data-bind attribute set to click: updateVisible. Here, the
button's click event is bound to the model attribute, updateVisible. The ViewModel
object for the previous markup is shown in the following code snippet:

```
var viewModel = kendo.observable({
isVisible: true,
updateVisible: function() {
this.set('isVisible', !this.get('isVisible'));
}
});
kendo.bind($('div#view'), viewModel);
```

The initial value for isVisible in ViewModel is set to true and hence the text,
Some Content here..., (inside the span) would be visible. When you click on the
toggleVisible button, it toggles the isVisible model attribute. This will either show
or hide the span element.

# # Using Source and Template binding with ViewModels to generate HTML content

In the previous recipe, data binding between View components and ViewModel attributes
was explored. Binding can be performed not only on the value of the form elements, but also
on the attributes, and also to hide or show the elements on the page. In this recipe, we will
look at the source and the HTML binding in combination.
Consider a dropdown (to select an element) that you want to populate it with data (option
tags). The data for these could be provided from a ViewModel object; use a template to
generate these option tags.