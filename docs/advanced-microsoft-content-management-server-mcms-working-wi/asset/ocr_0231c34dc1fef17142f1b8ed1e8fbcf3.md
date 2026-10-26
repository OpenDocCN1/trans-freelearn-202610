```
as WindowsIdentity;

try
{
    // Authenticate into update mode
    cmsContext.AuthenticateUsingUserHandle(ident.Token,
            PublishingMode.Update);
}
catch(Exception ex)
{
    // Dispose the CMS Context
    cmsContext.Dispose();

    throw new Exception("Failed to Authenticate user by handle. "
            + "The user may not have author permissions.", ex);
}

// Get the MCMS Posting object by path
_Posting = (Posting)cmsContext.Searches.GetByPath(strPath);

// Verify the posting was found
if (_Posting == null)
{
    // Dispose the CMS Context
    cmsContext.Dispose();

    throw new Exception("The posting was not found at path \""
            + strPath + "\";");
}

// Instantiate the Lightweight posting passing
// in the MCMS posting to the constructor.
LightweightPosting _LightweightPosting = new LightweightPosting(_Posting);

// Dispose the CMS Context
cmsContext.Dispose();

// Return the Lightweight posting
return(_LightweightPosting);
}
```

# Configuring IIS Security

As we are using the current user's Windows identity to authenticate the CmsApplicationContext, we should verify anonymous access to this Web service is disabled.

+   1. In the web.config file, verify the authentication element is set to:
+      `<authentication mode="Windows" />`
+   2. In the Internet Information Services (IIS) management console, right-click on the McmsAuthoringWebService virtual directory and select Properties.
+   3. Select the Directory Security tab.
+   4. Under Anonymous access and authentication control, select Edit.
+   5. Uncheck Anonymous access and check Integrated Windows authentication.
+   6. Click OK and close all opened dialogs.
+   7. Close the IIS management console.