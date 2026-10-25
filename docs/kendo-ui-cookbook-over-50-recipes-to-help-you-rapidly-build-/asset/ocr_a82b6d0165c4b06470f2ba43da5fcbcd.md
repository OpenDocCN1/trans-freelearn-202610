```
id="listViewContainer">
</ul>
<ul
data-role="actionsheet"
id="optionActionSheet">
<li>
<a data-action="option1">Favourite</a>
</li>
<li>
<a data-action="option2">Watch</a>
</li>
<li>
<a data-action="option3">Remove</a>
</li>
</ul>
</div>
```

The content of the ListView widget is populated using a DataSource object. The ActionSheet widget (ul with data-role=actionsheet) contains a list of actions that need to be shown when the user clicks on any of the elements in the ListView widget. Notice that each list element contains an anchor element with a data-action attribute. This attribute is used to specify the JavaScript function to be invoked when the user clicks on the action button.

The next step is to associate elements in the list with the ActionSheet widget. To populate the elements in the list, template can be used and the same can specify the reference to the ActionSheet widget:

```
<script id="listViewTemplate" type="text/x-kendo-template">
<a
data-rel="actionsheet"
href="\\#optionActionSheet">
#= data #
</a>
</script>
```

Here, the anchor's data-rel attribute is set to actionsheet, and the href attribute refers to the ActionSheet widget (id=optionActionSheet).