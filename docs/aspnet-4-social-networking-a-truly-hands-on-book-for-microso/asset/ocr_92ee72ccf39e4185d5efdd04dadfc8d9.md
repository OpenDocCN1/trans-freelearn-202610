```
```csharp
linkDeleteAlbum.Attributes.Add("OnClick", "javascript:return(confirm('Are you sure you want to delete this album?')); ");
linkDeleteAlbum.Attributes.Add("FolderID", litFolderID.Text);
linkViewAlbum.NavigateUrl += "?AlbumID=" + litFolderID.Text;
}
}
```
```

And finally, we have a method to handle our delete link's click event. This method calls into our presenter to handle the deletion of an Album.

```
```csharp
//Fisharoo/Web/Photos/MyPhotos.aspx.cs
protected void linkDeleteAlbum_Click(object sender, EventArgs e)
{
    LinkButton linkDeleteAlbum = sender as LinkButton;
    _presenter.DeleteFolder(Convert.ToInt64(linkDeleteAlbum.Attributes["FolderID"]));
}
```
```

# ViewAlbum

The ViewAlbum page is exactly the same as the MyPhotos page in that it uses the ListView to handle its rendering of the data. The only difference is that we use PEFile instead of PEFolder.

In the code behind we have a similar layout as the MyPhotos code behind. We load the photos for the given album. We also load some details about the album itself so that we can show things like the album's description and the like. There are a few button controls that we have our events hooked to so that we can handle things like navigation via the redirector class. We are showing only the main parts of the code below:

```
```csharp
//Fisharoo/Web/Photos/ViewAlbum.aspx.cs
protected void lvAlbum_ItemDataBound(object sender,
                                      ListViewItemEventArgs e)
{
    if(e.Item.ItemType == ListViewItemType.DataItem)
    {
        HyperLink linkImage = e.Item.FindControl("linkImage") as HyperLink;
        Literal litImageName = e.Item.FindControl("litImageName") as Literal;
        Literal litFileExtension = e.Item.FindControl("litFileExtension") as Literal;
        string pathToImage = "~/files/photos/" +
                             linkImage.NavigateUrl + "/" + litImageName.Text;
        linkImage.NavigateUrl = pathToImage + "_o." +
                                litFileExtension.Text;
```
```