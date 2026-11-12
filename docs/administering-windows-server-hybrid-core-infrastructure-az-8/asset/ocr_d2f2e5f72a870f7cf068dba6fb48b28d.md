+   - Federation or pass-through authentication is planned for user sign-in
+   - More than 100,000 objects in the Windows Server AD forest need to use the full SQL Server
+   - Group-based filtering is planned to be used, not just the filtering of domains or OUs

# AD health check tools

It is critical to have an operationally healthy AD instance before implementing directory synchronization through Azure AD Connect. Typical issues that will be flagged and can be remediated will be related to UPNs and proxy addresses. The IdFix tool will check for errors and allow for any remediation once identified.

In this section, we introduced the Azure AD Connect tool. We also looked at the AD health-check tools. In the following section, we will look at the hybrid authentication methods.

# Hybrid identity authentication methods

Hybrid identity requires an authentication method that supports the integration of AD and Azure AD. Using AD Connect, the following sign-on methods can be configured for users to enable hybrid identity in an organization:

+   - Azure AD Password Hash Synchronization (PHS)
+   - Azure AD Pass-Through Authentication (PTA)
+   - AD Federation Services

The following screenshot shows the sign-on methods and options that are available:

![](bbox=[0.2, 0.61, 0.8, 0.855])

Figure 4.3 – Cloud sign-on methods