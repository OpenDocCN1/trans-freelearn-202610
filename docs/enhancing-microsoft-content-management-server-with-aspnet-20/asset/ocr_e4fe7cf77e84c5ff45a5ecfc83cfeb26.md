It’s probably a good idea to hide the root channel from the site map, since it isn’t a visible part of the site’s hierarchy. There are a couple of situations where the root node may get included in the site map:

+   - The node being accessed is that of the root channel, `/Channels/`, itself.
+   - The node being accessed is a page or channel item located in the root channel. For example, it could be a home page (`/Channels/Home.htm`) or one of the top-level channels (`/Channels/tropicalgreen/`). Therefore, its parent is the root channel.

In both situations, we need to set the return value of GetParentNode() to be null in order to hide the root channel in the site map. Add the highlighted code as shown here:

```
public override SiteMapNode GetParentNode(SiteMapNode node)
{
    . . . code continues . . .

    if (parent == null)
    {
        // The parent is null, possibly because we are somehow
        // accessing the root channel itself.
        return null;
    }
    else {
        // Check to see if the parent channel is "/Channels/"
        if(parent.Path == "/Channels/" && RootChannel != "/Channels/")
        {
            // We don’t want "Channels" to be part of the site map
            return null;
        }
        else
        {
            // Create a site map node based on the parent channel item
            // and return it
            return GetSiteMapNodeFromChannelItem(parent);
        }
    }
}
```

The completed method looks as follows:

```
public override SiteMapNode GetParentNode(SiteMapNode node)
{
    // Get the current channel item
```