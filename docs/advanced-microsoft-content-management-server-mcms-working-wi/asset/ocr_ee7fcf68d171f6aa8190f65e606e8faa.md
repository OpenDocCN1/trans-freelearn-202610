```
```csharp
{
    // Get the bin
    bin = CmsHttpContext.Current.Searches.GetByPath("/Channels/RecycleBin/")
        as Channel;

    // Set the text of the button
    this.Text = "Recycle Bin";
}

protected override void PerformActionBehavior()
{
    base.GenerateStartupScriptActionJavascript();
}

public override bool Available
{
    get
    {
        if (bin != null)
        {
            return true;
        }
        else
        {
            return false;
        }
    }
}

public override string ActionJavascript
{
    get
    {
        if (this.Available)
        {
            return "WBC_openWindowOnFocus('" + this.UrlNewWindow + "', '" 
                + "WBC_winRecycleBin", "+ "WBC_UseDefaultSizing(window.top)";
        }
        else
        {
            return "";
        }
    }
}

protected override void OnInit(System.EventArgs e)
{
    base.OnInit(e);

    this.UrlNewWindow = Page.Request.ApplicationPath
        + "/Dialogs/RestorePosting.aspx?"
        + bin.QueryStringModeUpdate;
}
}
```
```

> Store the path (or its GUID—amend the code to use Searches.GetByGuid() instead to retrieve it if you do so) to the recycling bin as a key within the <appSettings> tag of the project's web.config file. In this way, you can change its path easily without having to recompile the code.