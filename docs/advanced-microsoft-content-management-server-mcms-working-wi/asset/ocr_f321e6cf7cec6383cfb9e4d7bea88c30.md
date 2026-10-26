revision dates. We want to display the list in descending order with latest revision first, so we set the Boolean to false.

Once we have retrieved and sorted the collection of revisions, we bind it to the DataGrid, dgPageRevisionsList. This is all done in the GetListOfRevisions() method, which we add directly below the Page_Load() event handler.

```
public void GetListOfRevisions()
{
    // Get a list of all revisions of the current posting
    PostingCollection pc = currentPostingVersion.Revisions(true, false);

    // Sort the revisions by descending order of the revision date
    pc.SortByRevisionDate(false);

    // Bind the collection to datagrid
    dgPageRevisionsList.DataSource = pc;
    dgPageRevisionsList.DataBind();
}
```

> Unless the revision history is cleared periodically, we could potentially have a large number of revisions for each posting. To clear the revision history, select the Tools | Clear Revision History... option in Site Manager.

# Programming the Button that Triggers the Page Reversion Process

The Revert button on the DataGrid triggers the page reversion process. There is one Revert button for each revision listed on the grid. Authors will click on the button of the version they wish to roll the posting back to. Here's how the reversion process works:

+   - First, the author clicks on the Revert button next to the selected revision.

+   - When the Revert button is clicked, the dgPageRevisionsList_ItemCommand() event handler is called. Within the event handler, we will retrieve the selected revision from the revisions collection and call several key procedures:
+     - A CopyPostingContent() method, which copies all placeholder content from the selected revision to the working version
+     - A CopyCustomProperties() method, which copies all custom property values from the selected revision to the working version

+   - Finally, we refresh the parent window to show the posting with its recently reverted content.

# Accessing the ItemCommand() Event Handler of the DataGrid

Let's start by working with the standard ASP.NET behavior of the DataGrid by editing the ItemCommand() event handler.

When the Revert button is clicked, the ItemCommand() event handler of the dgPageRevisionsList DataGrid is fired. This is where we will add the code that carries out the page reversion process. To access this event handler, open PageRevert.aspx in Design view. Within the properties window of the dgPageRevisionsList DataGrid object, click the Events button (it's the button with the yellow