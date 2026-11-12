+   * In the case of the SOAP and REST APIs, by default the latest version of your Custom Objects is visible. When using the SOAP API, header values can be passed in such calls to lock a particular package version if needed (search for PackageVersionHeader in Apex Developer's Guide). Alternatively the packaged version used by these APIs can be set at an org-configuration level.

# # Versioning application access through the Salesforce APIs

Under the Setup menu, search for API. You will find a setup page that allows you to download the WSDLs for one of the many Salesforce SOAP APIs. In the next section there are some versioning settings that determine which installed package version should be assumed during interactions with your packaged objects.

For example, when performing a describeSObject web service operation via the Salesforce Enterprise or Partner APIs, these APIs will return different custom fields depending on when new fields were added, as new versions of the package have been released over time. The default behavior will return all fields as the default is the current package version.

If this is important to Developer X or third-party solutions when using these Salesforce APIs, the subscriber administrator can specify an explicit previous version. If they do this, new fields available in future package versions will not be available unless this is changed. The following screenshot shows how these settings can be accessed on the API page under Setup:

![](bbox=[0.125, 0.565, 0.92, 0.75])

![](bbox=[0.115, 0.77, 0.925, 0.83])