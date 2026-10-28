# Tags.aspx

The display side of this page simply has a handful of repeaters — one repeater for each type of object to which we allow the tags to be attached. When we load our UI, the presenter sends the view a list of SystemObjectTagWithObjects objects. We iterate through this collection and for each object, we bind the specific collection of objects to its corresponding Repeater. In the case of Files, which is what we have plugged our controls into; we would take the Files collection and bind it to the repFiles Repeater. As we add our new user controls to other sections of the site, we can hook up to new sections in this page.

```
//Fisharoo/Web/Tags/Tags.aspx
<asp:Repeater ID="repFiles" runat="server">
  <ItemTemplate>
    <tr><td>
      <%#((PESystemObjectTagWithObjects)Container.DataItem).FileName%>
    </td><td>
      <asp:HyperLink runat="server" Text='<%# "Click to view album: " + ((PESystemObjectTagWithObjects)Container.DataItem).FolderName %>' NavigateUrl='<%# "~/photos/ViewAlbum.aspx?AlbumID=" + ((PESystemObjectTagWithObjects)Container.DataItem).DefaultFolderID %>' /> or
      <asp:HyperLink runat="server" Text='<%# "Click to view photo: " + ((PESystemObjectTagWithObjects)Container.DataItem).FileName %>' NavigateUrl='<%# "~/files/photos/" + ((PESystemObjectTagWithObjects)Container.DataItem).URL %>' />
    </td></tr>
  </ItemTemplate>
</asp:Repeater>
```

# Tags.aspx.cs

All that this file does on its own is hook up to its presenter by passing the route data that captures the tag name. When the presenter's Init() method is called, the presenter calls into the LoadUI() method of the view.