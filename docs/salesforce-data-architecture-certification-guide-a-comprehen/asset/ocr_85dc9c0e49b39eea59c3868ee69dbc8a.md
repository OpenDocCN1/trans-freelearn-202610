# Account Teams

**Account Teams** represent the Salesforce users that play a role in the Account. For example, when selling to or serving an Account, you may have Salesforce users in your organization that are responsible for managing the relationship, being a point of escalation, and so on.

Account teams provide a way of associating your Salesforce users to Accounts in addition to what’s offered by the sharing model that’s already been configured in Salesforce for your organization. For example, you may have a private Account, Contact, and Opportunity sharing model, as well as a Role Hierarchy set up, that has sales team members in the same place of that Role Hierarchy. By default, those users would not automatically have access to all Account records. They would need to be explicitly shared using a Sharing Rule or Manual Sharing, for example. Account teams provide a way of opening up access to Accounts and other objects (including Contact, Opportunity, and Case). This access can be specified on a per-Account Team Member basis.

The Salesforce entity responsible for this functionality is called AccountTeamMember. As well as lookups to User and Account, this object provides AccountAccessLevel, CaseAccessLevel, ContactAccessLevel, and OpportunityAccessLevel picklist fields. These fields can have the None, Read, and Edit values and can be used to set the respective access level.

> **Important note**
> The AccessLevel picklist values must be set to values higher than the **Organization-Wide Defaults (OWDs)** for the respective object. For example, AccountAccessLevel must be set to **Edit** if the OWD provides Public Read-Only by default.

A representation of the AccountTeamMember object in the data model is as follows:

![](bbox=[0.28, 0.65, 0.72, 0.95])