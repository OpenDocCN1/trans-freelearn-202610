```
+ m_StageAsUser, m_StageAsPwd);
    m_credentials = new NetworkCredential(m_StageAsUser,
    m_StageAsPwd, m_StageAsDomain);
}
else
{
    cmsContext.AuthenticateAsGuest();
    m_credentials = null;
}
catch
{
    WriteToLog("Error: Unable to authenticate with MCMS server.");
}
}
```

# Revealing Hidden Postings

Next, the application picks up the setting stored in the m_DoNotExportHiddenItems variable and uses it to set the CmsApplicationContext.SessionSettings.AutoFilterHidden property. When this is set to false, the CmsApplicationContext will reveal items set to be hidden when published (or channel items with the ChannelItem.IsHiddenModePublished property set to true). As a result, channel items that are hidden will be staged.

```
[STAThread]
static void Main(string[] args)
{
    ... code continues ...
    // Reveal all hidden postings?
    cmsContext.SessionSettings.AutoFilterHidden = m_DoNotExportHiddenItems;
}
```

# Staging Channels and Postings

The next step of the process is to generate static pages for each channel and posting that the 'Stage As' user can see. Here's a quick synopsis of what we will be doing:

+   - Get an instance of the start channel defined in the m_StartChannel variable.
+   - Iterate through all channels and postings, beginning from the start channel.
+   - For each channel and posting, issue an HTTP request.
+   - Get the response and save it to a file.

# Getting the Start Channel

Before walking through the list of channels and postings, we first need to get the start channel specified in the m_StartChannel variable. The iteration process begins from this channel.

Once we have an instance of the start channel, we are ready to begin the iteration process, triggered by the CollectChannel() method defined next.