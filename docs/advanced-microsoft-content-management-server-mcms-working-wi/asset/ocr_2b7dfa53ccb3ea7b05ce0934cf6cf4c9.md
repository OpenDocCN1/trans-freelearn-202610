Append the code highlighted below to the Main() routine:

```
[STAThread]
static void Main(string[] args)
{
    ... code continues ...
    // Start the iteration process
    Channel rootChannel = cmsContext.Searches.GetByPath(m_StartChannel) as Channel;
    if (rootChannel != null)
    {
        CollectChannel(rootChannel);
    }
    else
    {
        WriteToLog("Error: Start Channel " + m_StartChannel + " cannot be " + "found or user does not have sufficient rights!");
    }
}
```

# Iterating Through the Channel Tree

From the start channel, the CollectChannel() method walks through the rest of the tree, looking for channels and postings.

When it sees a channel, it will create a static version of the channel rendering script or default posting and all postings in its collection. The method calls itself recursively until all channels in the tree have been staged.

The Download() helper method will perform the task of downloading the content and generating static pages as we will see later. Add the CollectChannel() method to the code:

```
static void CollectChannel(Channel channel)
{
    // Download the channel rendering script or the default posting
    Download(GetUrlWithHost(channel.Url), channel.Path.Replace(m_StartChannel, ""), m_DefaultFileName, EnumBinary.ContentPage);
    // Download all the postings within the channel
    foreach (Posting p in channel.Postings)
    {
        WriteToLog("Info: Downloading Posting: " + p.Path);
        Download(GetUrlWithHost(p.Url), channel.Path.Replace(m_StartChannel, "/"), p.Name, EnumBinary.ContentPage);
    }
    foreach (Channel c in channel.Channels)
    {
        CollectChannel(c);
    }
}
```

The CollectChannel() method uses a helper function, GetUrlWithHost(), which replaces any host names found within the URL with the value stored in the m_SourceHost variable. The reason we do this is to handle links that do not have the host name as part of the URL, particularly for sites that do not have host header mapping turned on. For such sites, Channel.Url will return a value without the host header, like this: /plantcatalog/ficus.htm. To successfully issue the HTTP request that gets the page, the host value (which in our case will be http://www.tropicalgreen.net) has to be pre-pended to the URL.