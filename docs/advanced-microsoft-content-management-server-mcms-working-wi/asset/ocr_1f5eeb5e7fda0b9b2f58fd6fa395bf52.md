# Conventions

In this book, you will find a number of styles of text that distinguish between different kinds of information. This section provides some examples of these styles, and an explanation of their meaning.

There are three styles for code. Code words in text are shown as follows: "Pass the path of any object as an input parameter of the Searches.GetByPath() method to retrieve the requested object".

If we have a block of code, it will be set as follows:

```
HierarchyItem hlItem = cmsContext.Searches.GetByPath(
"/Channels/TropicalGreen/MysteryObject");

if (hlItem is Channel)
{
    // Object is a Channel
}
else if (hlItem is Posting)
{
    // Object is a Posting
}
```

When we wish to draw your attention to a particular part of a code block, the relevant lines will be made bold:

```
private CmsHttpContext cmsContext;
private void Page_Load(object sender, System.EventArgs e)
{
    // Put user code to initialize the page here
    cmsContext = CmsHttpContext.Current;
    if (!Page.IsPostBack)
    {
        GetPlantFactSheets();
    }
}
```

**New terms** and **important words** are introduced in a bold-type font. Words that you see on the screen, in menus, on dialog boxes, and so on, appear in our text like this: "clicking the Next button takes you to the next screen".

General tips, suggestions, or important notes appear in a box like this.

Troubleshooting tips and suggestions appear in the following format:

# The option for the component I want to install is grayed out and I can't select it.

That's because you do not have the prerequisites required by that component. For example, if you did not install Microsoft Visual Studio, you will not be able to install the Developer Tools. To find out what's missing, click on the component and read the description on the right-hand side of the dialog.