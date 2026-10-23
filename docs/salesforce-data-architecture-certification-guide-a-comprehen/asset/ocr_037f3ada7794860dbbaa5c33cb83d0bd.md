With Event Monitoring enabled, it is possible to understand events and act on them, either through the API or by visually looking at event types using a Tableau CRM app. When acting on the events using the API, it is possible to determine usage such as impossible journeys. Impossible journeys are logins or page requests close together in time yet from locations far apart, making access from those locations within a specific time impossible and potentially malicious.

You can read about how to enable Event Monitoring at `https://help.salesforce.com/s/articleView?id=000339868&type=1.`

# Session security settings

It’s possible to configure settings related to the security of a users session for when they are accessing Salesforce to safeguard access to your org data held on the platform.

Session settings cover a wide range of topics, including **timeout settings, caching, multi-factor authentication,** and **high-assurance sessions** for accessing sensitive data.

You can read more about session security at `https://help.salesforce.com/s/articleView?id=sf.admin_sessions.htm&type=5.`

# Field history tracking

Lastly (but by no means least), **field history tracking** is a powerful tool for understanding who made what change to a custom subset of object fields. Certain field changes may be required for tracking by your company – for example, who changes the **Stage** picklist on **Opportunities**, recording the old and new value, who made the change, and when. Field-level security allows for 20 fields per object to be tracked *out-of-the-box* with Salesforce, although Shield (which incurs a separate subscription/charge) can raise this limit to 60 fields per object. Additionally, with Shield, the field history data is archived after 18 months and stored for 10 years.

You can read more about field history tracking at `https://help.salesforce.com/s/articleView?id=sf.tracking_field_history.htm&type=.`

Now that we’ve looked at how to implement a compliant data model (looking at the various tools and techniques available on the Salesforce platform to do so), let’s turn our attention to enterprise data governance.

# Enterprise data governance

As data architects, we must look at the various approaches and considerations when designing an enterprise data governance strategy for implementation within an organization.

To understand this better, the following diagram shows the components of an enterprise data governance strategy: