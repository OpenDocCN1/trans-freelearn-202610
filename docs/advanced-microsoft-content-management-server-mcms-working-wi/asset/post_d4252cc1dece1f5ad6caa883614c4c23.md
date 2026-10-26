
## Staging Static Pages

```csharp
private static string m_DefaultFileName;
private static string m_DestinationDirectory;
private static string m_DefaultFileExtension;
private static string m_CodePage;
private static string m_BaseUrl;
private static string m_LocalBaseUrl;

// ArrayLists for handling attachments
private static ArrayList m_AttachmentUrls;

// CmsApplicationContext for working with the MCMS PAPI
private static CmsApplicationContext cmsContext;

// Network credentials for downloading pages
private static NetworkCredential m_credentials;
[STAThread]
static void Main(string[] args)
{
    - - - code continues - - -
}
```

Our application will handle two types of files:

- **ContentPage:** Postings and channel rendering scripts
- **ContentBinary:** Images, Office documents, and other attachments

We will declare an enumeration named EnumBinary to identify each of these file types. Add the following code above the Main() routine.

```csharp
// Have enum values for each file type handled
enum EnumBinary
{
    ContentBinary,
    ContentPage
}
```

Within the Main() routine, we will initialize each of the variables declared above. Go ahead and change their values. You will at the very least have to set the 'Stage As' user, domain, and password as well as the start channel and the destination directory to match the environment of your computer.

```csharp
[STAThread]
static void Main(string[] args)
{
    // Set parameter values
    m_LogFile = @"c:\StagedSite\logs\";
    m_StageAsDomain = "localmachine";
    m_StageAsUser = "StagerUser";
    m_StageAsPwd = "StagerPassword";
    m_DoNotExportHiddenItems = true;
    m_StartChannel = "/Channels/www.tropicalgreen.net/";
    m_SourceHost = "http://www.tropicalgreen.net";
    m_UserAgent = "Mozilla/4.0 (compatible; MSIE 6.0 Windows NT 5.1)";
    m_HttpHeader = "DotNetSiteStager";
    m_DefaultFileName = "default.htm";
    m_DestinationDirectory = @"c:\StagedSite\";
    m_DefaultFileExtension = "htm";
    m_CodePage = "UTF-8";
    m_BaseUrl = "http://tropicalgreen";
    m_LocalBaseUrl = m_BaseUrl;
    m_AttachmentUrls = new ArrayList();
}
```

## Recording Messages to a Log File

As the stager progresses through its job, it may encounter exceptions when staging certain URLs. Instead of terminating the entire process, we will trap all exceptions and write all error messages to a log file. The location of the log file is set in the m_LogFile variable defined earlier.

The helper function that writes messages to the log file is shown below. Add it directly below the Main() method.

```
private static void WriteToLog(string message)
{
    Console.WriteLine(message);
    FileStream fsLog = new FileStream(m_LogFile, FileMode.Append,
                                      FileAccess.Write);
    StreamWriter wLog = new StreamWriter(fsLog);
    wLog.WriteLine(message);
    wLog.Flush();
    wLog.Close();
    fsLog.Close();
}
```

For audit purposes, we will record the current date and time as well as the settings that we defined earlier in the log file. Within the Main() routine, add the following code:

```
[STAThread]
static void Main(string[] args)
{
    ... code continues ...
    // Ensure that the logfile directory is created
    Directory.CreateDirectory(m_LogFile);
    // Generate the name of the log file (in the format log_yyyMMdd_HHmm.txt)
    if (!m_LogFile.EndsWith("\\"))
    {
        m_LogFile = m_LogFile + "\\";
    }
    m_LogFile = m_LogFile + "log_" + DateTime.Now.ToString("yyyyMMdd_HHmm")
              + ".txt";

    System.Text.StringBuilder sb = new System.Text.StringBuilder();
    string newLine = System.Environment.NewLine;

    // Write the timestamp to the log file
    sb.Append(DateTime.Now.ToString("dd MMM yyyy HH:mm") + newLine);
    sb.Append("----------------" + newLine);

    // Write the settings to the log file
    sb.Append("Log File = " + m_LogFile + newLine);
    sb.Append("Stage As Domain = " + m_StageAsDomain + newLine);
    sb.Append("Stage As User = " + m_StageAsUser + newLine);
    sb.Append("Do Not Export Hidden Items = " + m_DoNotExportHiddenItems
              + newLine);
    sb.Append("Start Channel = " + m_StartChannel + newLine);
    sb.Append("HTTP Host = " + m_SourceHost + newLine);
    sb.Append("User Agent = " + m_UserAgent + newLine);
    sb.Append("HTTP Header = " + m_HttpHeader + newLine);
    sb.Append("Default File Name = " + m_DefaultFileName + newLine);
```

A better idea would be to store the settings within an Application configuration file (app.config). In this way, you don't have to re-compile the code each time any of the settings changes. For simplicity, we will leave them as class variables in this example.

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

## Revealing Hidden Postings

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

## Staging Channels and Postings

The next step of the process is to generate static pages for each channel and posting that the 'Stage As' user can see. Here's a quick synopsis of what we will be doing:

- Get an instance of the start channel defined in the m_StartChannel variable.
- Iterate through all channels and postings, beginning from the start channel.
- For each channel and posting, issue an HTTP request.
- Get the response and save it to a file.

### Getting the Start Channel

Before walking through the list of channels and postings, we first need to get the start channel specified in the m_StartChannel variable. The iteration process begins from this channel.

Once we have an instance of the start channel, we are ready to begin the iteration process, triggered by the CollectChannel() method defined next.

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

### Iterating Through the Channel Tree

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
