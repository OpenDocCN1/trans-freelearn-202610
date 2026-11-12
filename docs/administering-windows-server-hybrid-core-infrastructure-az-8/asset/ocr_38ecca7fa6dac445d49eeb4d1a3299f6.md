GPOs can be one of the following two types:

+   - **Local GPO**: Windows OS computers have policies whose settings only apply to the *local computer* and cannot be linked to any other object; they are applicable for computers that are *not AD joined*.
+   - **Domain GPO**: These are created and stored on AD domain controllers. These GPOs can then be linked to a *site*, *domain*, and *OU container* objects (not groups) and provide central management of configuration settings that wish to be applied to user and computer objects in the domain.

There are two default policies created when AD DS is installed on Windows Server, and these are as follows:

+   - **Default Domain Policy**: This is linked to the *domain container* and affects all computers. Security filtering settings can be applied to a specific security group; the default security filtering will apply to the *Authenticated Users* group.
+   - **Default Domain Controllers Policy**: This is linked to the *domain controllers OU* container for the exclusive purpose of applying domain-controller-specific settings.

You should create additional GPOs and link those to the site, domain, or OU container objects as required if you need additional settings that are unavailable in these GPOs.

In this section, we defined GPOs. In the next section, we will look at the GPO components.

# GPO components

An AD domain GPO consists of the following two components:

+   - **GPO Container**: This is an AD object and is a metadata store for the GPOs. The **Directory Replication Agent (DRA)** provides replication between DCs.
+   - **GPO Template**: This is a store for a set of files for the GPO. **Distributed File System Replication (DFSR)** provides replication between DCs.

For a GPO to be applied to a user or a computer, it must be linked to a site, domain, or OU container that contains computer or user objects that require to be affected by the policy; a GPO cannot be applied directly to a group. GPOs provide two groups of configuration settings, one for computers and one for users; there are no configuration settings for groups. The configuration settings nodes in any policy are as follows:

+   - **Computer Configuration**: The policies defined in this section will only affect computer objects. These settings are evaluated and applied at computer boot time.