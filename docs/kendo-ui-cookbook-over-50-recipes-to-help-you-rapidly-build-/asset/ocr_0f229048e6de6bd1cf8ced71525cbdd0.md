# # Kendo UI TreeView

```
```html
<ul>
    <li>File1.txt</li>
    <li>File2.txt</li>
</ul>
</li>
<li>
    Directory2
    <ul>
        <li>File3.txt</li>
        <li>File4.txt</li>
    </ul>
</li>
<li>
    Directory3
    <ul>
        <li>File5.txt</li>
        <li>File6.txt</li>
    </ul>
</li>
</ul>
```
```

The next step is to initialize the TreeView widget by invoking the `kendoTreeView` function on the DOM node using the following line of code:

`$('#treeView').kendoTreeView();`

# ## How it works...

The TreeView widget displays the hierarchical data in a tree-like structure, as shown in the following screenshot:

![](bbox=[0.422, 0.642, 0.608, 0.746])

Note that TreeView displays only the top-level elements. It shows you a right arrow icon before the element. On clicking on the icon, the top-level element expands to show you the next level in the hierarchical structure.