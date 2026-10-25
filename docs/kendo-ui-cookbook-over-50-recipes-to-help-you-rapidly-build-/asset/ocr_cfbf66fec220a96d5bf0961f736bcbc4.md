```
checkChildren: true
}
});
```

Now, when you select the parent node, all child nodes within the parent node would be selected. It is also possible to specify nodes as checked when they are rendered. In the DataSource configuration, specify checked as true:

```
$('#treeView').kendoTreeView({
 checkboxes: {
 checkChildren: true
 },
 dataSource: [{
 text: "Directory1",
 items: [
 {text: "File1.txt"},
 {text: "File2.txt"}
 ],
 checked: true
 }
 .
 .
 ]
});
```

# How it works...

When you set the checkboxes property as true, you would see a checkbox before each node in TreeView.

![](bbox=[0.432, 0.701, 0.592, 0.904])