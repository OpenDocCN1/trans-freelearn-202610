
Managed Service Accounts (MSAs) are AD DS object classes that provide this service account capability; they benefit from lowering admin effort for Service Principal Name (SPN) management and service account password management.

There are two types of MSAs, as follows:

- Standard Managed Service Accounts (sMSAs) are local to a computer, service, or application and cannot be shared across other resources in the network. This limits their efficiency and adds an administrative burden. Needing to implement multiple local sMSAs increases the threat, security, and configuration drift surface area and exposure.

Further information about sMSAs can be found at https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/service-accounts-standalone-managed.

- Group Managed Service Accounts (gMSAs) have domain scopes and capabilities and can be used across multiple computers, services, and applications; NLB clusters and IIS are examples.

gMSAs require you to create a key distribution services (KDS) root key on a DC to create gMSA accounts.

The following are some AD PowerShell cmdlets that can be used to manage gMSAs:

- Get-ADServiceAccount
- Install-ADServiceAccount
- New-ADServiceAccount
- Remove-ADServiceAccount
- Set-ADServiceAccount
- Test-ADServiceAccount
- Uninstall-ADServiceAccount

Further information about gMSAs can be found at https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/service-accounts-group-managed.

## Computer objects

Just as users have an account in the directory service so that we can search for and locate these users, we need the same mechanism to search for and locate computers in the domain. A computer object provides an account for a computer; it represents a computer resource on the network and allows it to uniquely identify itself and authenticate. Once a computer has a computer account, it can be managed as a domain resource and configured using Group Policy.

User account objects and computer account objects are the two foundation building block object classes in AD DS.

## Group objects

Group object classes in the directory form the basis for access management.

Once you have been authenticated through your user account, your group object membership defines what you are authorized to access in the domain. The permissions assigned to the group define what actions you can carry out on that network resource.

Permissions shouldn’t be set at the user account level; this is an admin burden, inefficient, prone to configuration drift, and becomes a governance and security posture issue.

All permissions should be assigned to a group, and users should be made members of the appropriate groups to give them the required permissions. This makes it easier to govern and control policies such as joiners, leaver policies, or where people change roles and now require different access levels. Rather than change individual resource permissions directly for what they can now do/not do based on their new role definition, we can add or remove them to the appropriate groups based on their new role’s permissions requirements. This is much more efficient, less error-prone, and provides much better control and governance.

Adding user accounts to groups is an RBAC approach. The principle of least privilege should be adopted, meaning you only need just enough access to perform the tasks/activities required without the need for elevated permissions. This should be the cornerstone of protecting against lateral attacks following an account compromise.

In summary, by adopting the practice of creating groups, assigning permissions to groups, and then making user accounts members of those groups, we can efficiently manage access to resources for multiple users that all need the same access type.

## Group types

There are two types of groups: security groups and distribution groups. Let’s look at these in more detail:

- Security groups are used as security-enabled logical containers to collectively group users to assign access permission to resources. This is a much more efficient way to assign a set of permissions to resources than assigning them individually to each user. For example, to control access to a network resource, we would create any number of groups, each with different levels of access permissions. Then, users can be added to the appropriate group(s), depending on what level of access they should have. We should ensure we follow the least privilege approach when assigning users to groups – that is, if they only need read access, then don’t make them members of a group with full-control permissions.

Two default security groups are created automatically when AD DS is installed; these groups are located in the Builtin and Users containers.

- **Distribution groups** are used as *non-security enabled* logical containers, that is, no permissions can be assigned to these groups, and are used for grouping users for providing some other form of non-security related administration, and to target a subset of users, often a business unit, team, or project focus. Most commonly used for email applications, for sending emails to target all group members.

## Group scopes

Groups also have a *scope*. This defines where they can exist/be effective, the types of members that can be added, and the capabilities and permissions that the group supports.

Four types of group scopes are supported in Windows Server: *local groups*, *domain-local groups*, *global groups*, and *universal groups*. Let's look at these in more detail:

- **Local groups** are effective only on the local machines they are created on and cannot be created on DCs; for clarity, these are not domain objects and exist only on local machines. The following are characteristics of local groups:
  - **Abilities and permissions**: Can be assigned to local (non-domain) resources only
  - **Members**: Any user account of the forest

- **Domain-local groups** only apply locally to the domain created within and exist on the AD DS DCs for that domain. They are the primary means of managing access rights and responsibilities to resources within the local domain. The following are characteristics of domain-local groups:
  - **Abilities and permissions**: Can only be assigned to domain-local resources
  - **Members**: Any user account of the forest

- **Global groups** are used to collectively group user accounts that share similar functions; this could be based on geographic location or business unit. The following are characteristics of global groups:
  - **Abilities and permissions**: Can be assigned to any forest resource
  - **Members**: Local domain only

- **Universal groups** are used in multi-domain environments and combine the functions of the domain-local and global groups. The following are characteristics of universal groups:
  - **Abilities and permissions**: Can be assigned to any forest resource
  - **Members**: Any user account of the AD DS forest

You set the group type and scope at the time of creation and choose based on your needs and requirements for each group; this must be given adequate planning.

This section looked at AD DS objects. The next section will look at the AD DS domain and domain tree topology.

## Domains

As inferred by the name **Domain Services**, a domain is the core foundation component and the building block of AD DS. A domain is a logical container of objects in the directory for management purposes. AD DS directory objects could be user accounts, computer accounts, groups, printers, and so on; all objects within that domain are bound by and share the same admin and security policies.

The domain is an *administrative* and *replication* boundary for AD objects; security boundaries are only implemented at the forest level. We will explore this in the *Forests* section.

A domain has one or more DCs. This is a Windows Server role responsible for authenticating requests to access resources in the domain, such as authenticating and authorizing a login request to a computer or accessing a storage file share. The DCs hold a copy of the AD DS database that is writeable; we will look at the concept of RODC and its use cases later.

Domains have a hierarchal topology, referred to as a domain tree. It consists of domains in a parent/child relationship that share a parent root domain; they also share a contiguous namespace in DNS.

The AD DS domain and domain tree are shown in the following diagram:

![](img/cf35f46a8dd5f11e58349c1d81611b75_36_0.png)

Figure 1.5 – Domain topology

AD DS uses a *multi-master replication model*, meaning that any DC can make changes to objects in the directory stored in the AD database. When changes to the domain are made on a DC, such as adding, removing, or changing an object, these changes are replicated to all other DCs in the domain. In a multi-domain case, only a subset of changes is replicated to all domains.
