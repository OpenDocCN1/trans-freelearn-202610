
- 16. What is the order of GPO processing?
- 17. Explain inheritance, blocking, and enforcement.
- 18. What is the primary GUI management tool?
- 19. List the CLI tools that can be used.
- 20. What attributes does a GPO have?

## Implementing and Managing Hybrid Identities

This chapter covers the AZ-800 Administering Windows Server Hybrid Core Infrastructure exam learning objective: _Implement and manage hybrid identities_.

In the previous chapter, we added skills to understand the concepts, creation, and management of Group Policy.

We will start this chapter with some concepts and an understanding of hybrid identity. We will then look at the AD Connect directory synchronization tool that is the enabler for a hybrid identity and the hybrid identity authentication methods available. We will then conclude with a hands-on exercise to develop your skills further. The following topics will be covered in this chapter:

- What is a hybrid identity?
- Azure AD Connect
- Hybrid identity authentication methods
- Exercise – Implementing Azure AD Connect

In addition to the topics listed in this chapter, this chapter’s goal is to take your knowledge beyond the exam objectives to prepare you for a real-world, day-to-day hybrid environment-focused role.

### What is a hybrid identity?

In this first section of this chapter, we will recap some definitions and concepts to set a baseline and foundation of knowledge to build from.

Microsoft provides two **identity provider (IDP)** and **Directory Service (DS)** solutions; **Active Directory (AD)** and **Azure Active Directory (Azure AD)**.

In a nutshell, **AD** is an **IDP** *dedicated* to a *single organization*. It runs as an installed service as part of Windows Server.

Azure AD, in a nutshell, is Microsoft's multi-tenant, cloud-based, and fully managed IDP and DS. Azure AD can be considered Microsoft's **Identity as a Service (IDaaS)**.

A **hybrid identity** provides users with a *common identity*. This is simply a means to provide users access to resources and services regardless of whether they are cloud-based or on-premises. This approach means less confusion for users and one identity that can be used independently of location or IDP. This can be visualized in the following illustration:

![](img/cf35f46a8dd5f11e58349c1d81611b75_128_0.png)

Figure 4.1 – AD and Azure AD integration

In this section, we provided a recap of Microsoft's IDPs, AD, and Azure AD, which we covered in the first two chapters, and defined what a hybrid identity is. The following section looks at the Azure AD Connect tool, the foundation for enabling hybrid identity authentication methods.

### Azure AD Connect

**Azure AD Connect** is a free download tool enabling *hybrid identities* and seamlessly connecting and integrating AD with Azure AD.

Azure AD Connect is implemented by deploying the service to Windows Server, usually to a dedicated server. However, depending on your needs, it could also be run on a domain controller.

The following are the operations of AD Connect when installed on the chosen Windows server:

- Users, groups, and contact objects that exist or are added to AD configured for directory synchronization are copied to Azure AD through a synchronization process using Azure AD Connect. Note that users have no automatic license assignment, such as M365.

- Any changes to an object's attributes in AD will be updated to the synchronized object in Azure AD. The *Synchronization Service Manager* (the `miiscclient.exe` process) of Azure AD Connect allows you to be selective about what attributes are synchronized, so not all are copied if required.

- Any deleted users, groups, and contacts from AD are also deleted from Azure AD.

- The single source of authority for objects comes from AD as the *authoritative IDP* and not Azure AD. This means any changes required in the Azure synchronized objects must have their attributes and changes made in AD and allow those changes to be updated in the Azure AD objects by the synchronization process.

- The previous point means that Azure AD objects are *read-only* and cannot be changed directly in Azure AD unless a writeback solution is implemented for password resets in Azure AD. **Self-service password reset (SSPR) writeback** is an optional feature that allows password changes to Azure AD, leading to the same identity having different passwords between IDPs. This can be solved with password writeback, so password changes made in Azure AD can be sent back to AD to update the objects attribute so that the same password is maintained in both AD and Azure AD directories.

### Azure AD Connect cloud sync

Similar capabilities of Azure AD Connect are also available in the **Azure AD Connect cloud sync** service.

This is based on a *provisioning agent* approach rather than a fully installable application. The configuration and orchestration are handled from Azure AD rather than from AD itself. All information is stored within Azure AD rather than locally, where the AD Connect tool is installed and running. The tool has a use case for mergers and acquisitions where many multiple disconnected and isolated forests are required to be integrated with Azure AD. Multiple provisioning agents can be deployed to provide high availability for directory synchronization.

### Azure AD Connect planning

The following planning activities must be considered before using the AD Connect tool. Planning is everything when it comes to implementing a hybrid identity:

- Identifying a domain-joined server to download and install the Azure AD Connect tool and components of the service; based on needs, the tool can be installed directly on a DC server in the domain to be synchronized or on a dedicated server. The following are the supported Windows Server OS versions:

- Windows Server 2016 or later; standard or datacenter edition.

- Windows Server Essentials 2019.

- The server where AD Connect will be installed must run a full GUI; Windows Server Core is not supported.

- Identifying schema extensions and custom attributes.

- Identifying domain functional levels.

- Identifying the preparedness of AD with regards to AD object attributes and User Principal Names (UPNs). If you have a non-routable domain, such as a local one, that is used for user UPNs; then you should use a domain that can be validated with Azure AD – for instance, a valid internet domain, such as .com or .net.

You then need to set the UPN suffix for users to use this Azure AD-verified domain. You could change the primary domain of your Azure AD DS domain if you wished to synchronize to Azure AD, but a better approach would be to add a UPN suffix to the verified domain you wished to add to Azure AD. This change is made in the **Active Directory Domains and Trusts** management console from the **UPN Suffixes Properties** tab. The following illustration shows where this configuration is done:

![](img/cf35f46a8dd5f11e58349c1d81611b75_130_0.png)

Figure 4.2 – Adding a UPN suffix

This topic will be covered in the hands-on exercise section of this chapter.

You can add multiple UPN suffixes and add and verify them in Azure AD if this meets your needs:

- 1. Validate that you own the chosen domain(s) to add to Azure AD for directory synchronization and ensure you have access to DNS records to make a change. This is to be able to verify the domain(s) when adding the domain to Azure AD.

- 2. Once any non-routable domains for synchronization have been addressed and prepared, you can proceed to add and verify the user's AD DS UPN suffix domain(s) to Azure AD for use with directory synchronization and single sign-on (SSO). Then set this domain in Azure AD as the primary domain for the tenant, that is, you should not use the tenant's default .onmicrosoft.com domain.

### Staging mode

AD Connect can also be used in staging mode, allowing changes to be made to a configuration and previewed before the server is made active. It also allows pre-production testing by verifying that all changes will be made as expected in production when you run a full import and synchronization through staging mode. Staging mode can be used in the scenarios of high availability, testing, the deployment of new changes to the configuration, and decommissioning or replacing servers.

Staging mode can be selected during the installation of AD Connect but does not run any exports. Staging mode does not run password sync or writeback. When staging mode is disabled, the exports will run, and password sync and writeback will be enabled.

### Express and custom installation

The express installation method was designed to support a configuration that worked for most customers most of the time, and new installations use this about 90% of the time.

The express installation assumes the following:

- A single Windows Server AD forest
- An enterprise administrator account that can be used for the installation
- Less than 10,000 objects in the Windows Server AD forest

The express installation provides the following configuration outcomes:

- Password hash synchronization from Windows Server AD to Azure AD
- Synchronization of all eligible objects in all domains and OUs

The custom installation method is used when your scenario does not match the preceding assumption and configuration outcomes.

The custom installation assumes the following:

- No access to an enterprise administrator account
- More than one Windows Server AD forest to be synchronized
- Forest domains that are unreachable from the Connect server
