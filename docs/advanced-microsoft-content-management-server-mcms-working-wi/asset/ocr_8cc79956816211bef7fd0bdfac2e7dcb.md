5. Now let's get an instance of the template (or the original template) on which to base the connected template. Before we do so, first consider how template designers and administrators create connected templates from CMS Explorer. They will:

+   - Click the Templates button.
+   - Navigate to the template gallery that contains the original template.
+   - Click the Edit link next to the original template.
+   - Select the Create Connected Template link.

![](bbox=[0.195, 0.26, 0.795, 0.41])

Coded within the Create Connected Template link is the path to CreateConnectedTemplate.aspx and a querystring parameter named CMSObjectGuid, which contains the GUID of the original template. Here's a sample of the generated URL, split on to several lines for readability:

```
http://localhost/cmsexplorer/CreateConnectedTemplate.aspx?
NRMODE=Update
&FRAMELESS=true
&CMSObjectGuid=%7b5010EF8C-C33C-4F68-8430-D8DFAABA2EE0%7d
&NRNODEGUID=%7bE4D19123-9DD3-11D1-B44E-006097071264%7d
```

In order to get an instance of the original template, we will extract the GUID from the CMSObjectGuid querystring parameter and pass it as the input parameter for the Searches.GetByGuid() method.

To show that we really have an instance of the original template, we will display the path of the original template on the screen. If, for some reason, we are not able to get an instance of the template, an error message will be displayed instead. We'll add the code that does the job next.

+   1. Double-click anywhere on the web form to get to its code-behind file. Add the Microsoft.ContentManagement.Publishing namespace.
+      - - - code continues - - -
+      // MCMS API
+      using Microsoft.ContentManagement.Publishing;
+      namespace CmsExplorer
+      {
+      - - - code continues - - -

+   2. Above and within the Page_Load() event handler, add the highlighted code:

```
private CmsHttpContext cmsContext;
private Template originalTemplate;

private void Page_Load(object sender, System.EventArgs e)
```