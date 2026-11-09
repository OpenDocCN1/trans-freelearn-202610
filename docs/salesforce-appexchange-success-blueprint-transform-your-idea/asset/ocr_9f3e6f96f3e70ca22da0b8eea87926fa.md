address security concerns highlighted by individual developers can be impractical, especially when these APIs are integral to enhancing the functionality of Salesforce apps.

As per Salesforce’s *Security Review Overview*⁶, even if ISVs don’t directly own the external endpoints or services their app integrates with, they remain within the purview of the security review. Salesforce adopts a holistic approach, ensuring every component of an offering is tested to safeguard customers and their data. This encompasses any third-party web applications or services linked with the app. Moreover, ISVs must secure written consent from third parties, authorizing Salesforce to test their endpoints.

6 https://partners.salesforce.com/pdx/s/learn/article/isv-security-review-overview-MC4QVWO6S2FVFD5MNRMLIEF3UBGQ?language=en_US#media

For situations where this approach seems overly stringent, it’s advisable for partners to reach out to Salesforce directly, perhaps through an office hours call. This can facilitate discussions on potentially excluding certain external systems from the review scope and instead providing a declaration in the submission.

# Salesforce’s trusted list

Salesforce maintains an unofficial roster of public APIs deemed secure based on previous evaluations. If an app integrates with one of these APIs, there’s no need to provide Chimera/ZAP scan evidence during the security review. API providers such as Stripe and Google are examples of those included in this list. For instance, if your app leverages the Google API for address validation, you won’t need to perform security scans on Google’s endpoints.