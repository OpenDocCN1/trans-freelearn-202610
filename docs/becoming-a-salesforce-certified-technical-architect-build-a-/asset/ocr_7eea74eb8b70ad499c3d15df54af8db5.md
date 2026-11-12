# Your integration interfaces table will now look as follows:

| Interface Code | Source/Destination | Integration Layer | Integration Pattern | Description | Security | Authentication |
|---|---|---|---|---|---|---|
| Int-001 | Public Website -> Salesforce | Data | Remote Call-in | Using web-to-lead to capture leads. Leads contain info such as company information, office addresses, phone numbers, and the inquiries from the prospective customers. | HTTPS (one-way SSL/TLS) | None, implementing a DOS mechanism (such as CAPTCHA) is recommended |
| Int-002 | IoT sensors -> Heroku | Data | Remote Call-in | Sensors send regular status updates to Heroku roughly every 30 seconds. | HTTPS (one-way SSL/TLS) | OAuth 2.0 asset token flow |
| Int-003 | Salesforce <-> Heroku | Data | Batch data sync – async | Bi-directional data sync between Salesforce Core Cloud (Force.com) and Heroku using Heroku Connect. Assumed objects: account and cases. | HTTPS (one-way SSL/TLS) | OAuth 2.0 web server flow |
| Int-004 | Tablet app -> MS SharePoint Online | Data | Remote process invocation – request and reply | Upload documents from the tablet app to SharePoint Using SharePoint APIs. | HTTPS (one-way SSL/TLS) | OpenID Connect user-agent flow |
| Int-005 | Tablet app -> Salesforce | Data | Remote Call-in | Pull data from Salesforce such as customer information and ID. | HTTPS (one-way SSL/TLS) | OpenID Connect user-agent flow |
| Int-006 | MS SharePoint Online <> Salesforce | Data | Batch data sync – async | Using Files Connect, retrieve links to documents stored in SharePoint and display them in Salesforce. | HTTPS (one-way SSL/TLS) | OpenID Connect web server flow |
| Int-007 | Salesforce <> MuleSoft <> ERPs | Data | Batch data sync – async | A scheduled batch job to sync customer and order data from Salesforce to the ERPs. The updates of these records should be synchronized back from the ERPs to Salesforce. The frequency of the scheduled job is a function of the volume of changes and is assumed to be 15 minutes. | HTTPS (two-way SSL/TLS) between Salesforce and MuleSoft. HTTPS (one-way SSL/TLS) Between MuleSoft and the ERP. | OpenID Connect JWT flow |
| Int-008 | Mobile app -> Salesforce | Data | Remote process invocation – request and reply | Retrieve and update customer data such as profile using the Salesforce REST API. | HTTPS (one-way SSL/TLS) | OpenID Connect user-agent flow |
| Int-009 | Salesforce <> Marketing Cloud | Data | Batch data sync – async | Sync data such as contacts and leads from Salesforce to Marketing Cloud and vice versa using the Marketing Cloud Connector. | HTTPS (one-way SSL/TLS) | OAuth 2.0 client credentials flow (to authenticate to Marketing Cloud) OAuth 2.0 web server flow (to authenticate to Salesforce) |
| Int-010 | Heroku -> Tableau CRM | Data | Batch data sync – async | Copy aggregated sensors' readings data from Heroku to Tableau CRM. | HTTPS (one-way SSL/TLS) | OpenID Connect web server flow |
| SSO-001 | SSO for MS SharePoint using Azure AD as IDP | SSO | SSO – SAML 2.0 | SSO using SAML 2.0 with Azure AD as an IdP SP-initiated flow. | HTTPS (one-way SSL/TLS) | SAML 2.0 SP-initiated flow |
| SSO-002 | SSO for Salesforce using Azure AD as IdP | SSO | SSO – SAML 2.0 | SSO using SAML 2.0 with Azure AD as an IdP SP-initiated flow. | HTTPS (one-way SSL/TLS) | SAML 2.0 SP-initiated flow |
| SSO-003 | SSO for ERP systems using Azure AD as IdP | SSO | SSO – SAML 2.0 | SSO using SAML 2.0 with Azure AD as an IdP SP-initiated flow. | HTTPS (one-way SSL/TLS) | SAML 2.0 SP-initiated flow |

Figure 5.7 – Integration interfaces (final)

That concludes the first mini scenario. You can try to re-solve this scenario as many times as needed. It is recommended to attempt solving it with no time restrictions the first time, with the aim of creating the best possible artifacts and presentation pitches.

You can then try to add some time restrictions, such as a 90-minute limit, to create your end-to-end solution.