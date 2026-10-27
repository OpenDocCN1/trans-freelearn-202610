# Adding media type to stylesheets

Sometimes a visitor of the page does not use the special print button but the default print dialog of the browser. In this case, the whole page is printed out, as shown on the screen. You can use media type for the stylesheets to customize the printed version in this case as well. In pure CSS, you can define different media types as HTML attributes in the link tags as follows:

```
<link rel="stylesheet" type="text/css" media="screen" href="sheet.css">
<link rel="stylesheet" type="text/css" media="print" href="print.css">
```

In CMS Made Simple, you add the Smarty tag {stylesheet} to the template that generates the HTML link tags automatically, so that you cannot add HTML attribute media to it. Nevertheless, you can attach two different stylesheets to your template and mark them as screen, print, or any other allowed type.

In the admin console, open your stylesheet (Layout | Stylesheets) for editing and scroll down to the Media Type section:

![](bbox=[0.191, 0.45, 0.812, 0.67])

Select the media types that you would like to serve with this stylesheet, for example, screen. To add a special stylesheet containing formatting for the print view of your page, create a new stylesheet with print formats. Select print as the media type in the Media Type section (see the preceding screenshot), then attach this stylesheet to the template. CMS Made Simple will automatically add the HTML attribute media to the link tags generated at the place where {stylesheet} was added to the template.