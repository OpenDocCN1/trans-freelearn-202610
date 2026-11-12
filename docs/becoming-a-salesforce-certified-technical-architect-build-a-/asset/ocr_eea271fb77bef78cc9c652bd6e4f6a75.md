The following sequence diagram (a modified version of the SAML SP-initiated flow in Chapter 4, Core Architectural Concepts: Identity and Access Management) explains this flow. Please note that this diagram is simplified and does not describe in detail the way Kerberos works:

![](bbox=[0.15, 0.17, 0.84, 0.58])

# Figure 13.4 – SAML SP-initiated flow with Kerberos

Be prepared to draw and explain this sequence diagram, similar to any other authentication flow you covered in Chapter 4, Core Architectural Concepts: Identity and Access Management.

Update your landscape diagram and your integration interfaces by adding another SSO interface, then move on to the next requirement, which starts with the following:

PPA employees can log in to Salesforce from outside the corporate network using the LDAP credentials.

The good news is that both requested features are supported by tools such as PingFederate. PingFederate has a feature called adaptive authentication and authorization, which delivers a set of functionalities, including reading the user's IP address to determine whether the user is inside or outside the corporate network and driving different behavior accordingly.