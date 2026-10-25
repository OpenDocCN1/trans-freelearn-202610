```
value: "20px"
}
]
}
]
});
```

In the preceding code snippet, the tools configuration contains an array of objects. Each object
has a name property, which specifies the name of the tool to be used and an items array,
which lists the values to be displayed in the dropdown. Consider the items array used for the
fontName tool. It contains a collection of objects, each having two properties, namely text
and value. The text property specifies the text to be displayed and the value property
specifies the value to be applied when the user selects the same value from the list. In this
case, if the user selects Default from the list then the Arial, Verdana, sans-serif font would
be applied.

![](bbox=[0.1162, 0.3672, 0.8916, 0.5852])

In many cases, you would like to create your own custom command that interacts with the
Editor interface. For example, a custom command that would insert text into the Editor. Apart
from various tools that the user can add to the toolset, the library allows you to define custom
commands that interact with the Editor:

```
$("#editor").kendoEditor({
    tools: [{
        name: "custom",
        exec: function(e) {
            var editor = $(this).data('kendoEditor');
            editor.exec("inserthtml",
                {value: "Thanks,<br>Sagar Ganatra."});
        }
    }]
});
```