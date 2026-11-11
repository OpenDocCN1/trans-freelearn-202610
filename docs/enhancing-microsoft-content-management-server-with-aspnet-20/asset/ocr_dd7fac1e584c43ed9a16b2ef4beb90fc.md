```
if (Ticket != null)
{
    HttpContext.Current.Items.Add("MCMSTicket", Ticket);
    ValidUser = true;
}

return ValidUser;
}
```

The Items collection is a bucket that can be used to pass all kind of objects between different code parts within one request—even between different HttpModules. The lifetime of the HttpContext Items collection is a single request. We can now utilize the MCMS ticket created by the Membership Provider within the Login control.

Now let’s implement a Base Membership Provider class for MCMS. We will do this by creating a new Class Library Project rather than using the App_Code folder. Once complete, this can be reused across multiple MCMS applications.

By implementing a base Membership Provider, we can separate the code needed to interact with the MCMS authentication and authorization system from code used to manipulate credential format or interact with external authentication systems.

+   1. Open the TropicalGreen website in Visual Studio 2005.
+   2. Choose Add from the File Menu, followed by New Project...
+   3. In the Add New project dialog, select the Class Library icon in the Visual Studio Installed Templates section.
+   4. Enter MCMSMembershipProviders in the Name textbox, and click OK.
+   5. Delete the Class1.cs file.
+   6. Right-click References and click Add Reference...
+   7. In the Add Reference dialog, select the System.Configuration and System.Web components and click OK.
+   8. Right-click References and click Add Reference... In the Add Reference dialog, click the Browse tab.
+   9. Navigate to the c:\Program Files\Microsoft Content Management Server\Server\bin folder, select the Microsoft.ContentManagement.Web.dll file, and click OK.
+   10. Right-click the MCMSMembershipProviders project and click Add, followed by New Item...
+   11. In the Add New Item dialog, select the Class icon and enter MCMSMembershipProviderBase in the Name textbox. Click OK. Add the following using statements: