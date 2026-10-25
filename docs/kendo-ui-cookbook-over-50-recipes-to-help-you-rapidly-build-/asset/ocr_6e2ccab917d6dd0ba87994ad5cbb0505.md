```
{
    text: "Flights",
    contentUrl: "PanelBar_Flights.html"
}
]
```

Here, the contentUrl option specifies the URL from which the content for the panel should
be fetched. When the user clicks on the panel, a request is sent to the specified URL and the
HTML content received as a response is then inserted into the panel.

# How it works...

When you render the page, the PanelBar is populated with the data that you have specified
in DataSource. When you check the HTML markup inserted into DOM, you will see the class
specified in the cssClass attribute added to each list element. Also, an image is inserted
inside each list element as follows:

```
<li role="menuitem" class="k-item k-state-default k-first
destinationData">
    <span class="k-link">
        <img class="k-image" alt=""
        src="./images/India-Flag-icon.png">
    India
    </span>
</li>
```

# Customizing the PanelBar

There are a few options available to customize the behavior of the PanelBar widget. In this
recipe, we will take a look at setting the expanded mode for the PanelBar widget and the
animation effects that can be applied.

# How to do it...

The PanelBar widget is designed so that it occupies less space on the page and, at the same
time, shows you the necessary content. When you click on any of the list items in the PanelBar
widget, it expands to show you the content. Now, when you click on the other list item, it would
also expand to show its content. At this stage, there are two items in the PanelBar whose
content is being shown. By setting the expandMode property to single, only one item in
the list can remain in the expanded state at any given time:

```
$("#panelBar").kendoPanelBar({
    expandMode: 'single'
});
```