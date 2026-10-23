# Understanding Salesforce sharing and security

Any data access in Salesforce is determined by a layered security model. With each layer, data access is gradually opened up. Therefore, it is imperative that at the lowest level the sharing and security requirements are well understood in order to configure the security settings as appropriate. The layers of the Salesforce security model can be visualized as follows:

![](bbox=[0.13, 0.23, 0.84, 0.58])

Figure 2.6 – A visual representation of the layers of the Salesforce security model

The layers that make up the security model, how they are configured, and their impacts are explained in this section.

# Organization-Wide Defaults

The access that users have to each other's records by default is specified in the Organization-Wide Defaults (OWDs) for a Salesforce instance. Essentially, OWDs lock data access down to the most restrictive level, and then the other sharing and data access tools (explained in the following sections) are used to gradually open up data access to your users. For example, setting a read-only account OWD will mean that users cannot edit accounts by default. If users require the ability to only edit accounts they own, grant edit and read access at the object level through the use of a profile or permission set, leaving the OWD set to read-only for accounts. There are separate OWDs for internal and external (through a Salesforce Community or otherwise) users. The premise of access is the same, but the external OWDs facilitate locking down to a more restrictive data access model by default. For example,