The default built-in group schema administrators control access to the schema; you must be a member of this group to make changes to the schema.

In this section, we looked at the AD schema. In the next section, we will look at AD DS DCs.

# Domain controllers

A DC is a server with the AD DS role installed; it holds a writeable copy of the AD database and functions through the multi-master replication model for this information store. It also provides a store for GPOs. It ensures that the configuration settings are applied to domain-managed objects.

The DC is also a Kerberos **Key Distribution Center (KDC)**. Kerberos is the mechanism for all authentication and authorization within AD; a Kerberos key identifies an object (authentication – that is, who you are) and outlines what resources an object can access (authorization – that is, what you can do and access).

DCs must be highly available to process logon requests; they are required to authenticate requests for access to domain resources and apply policies for managing and configuring objects in the directory stored in the AD database.

All computers and users that wish to access objects in an AD DS domain must be authenticated by a DC, so their placement, operation, and availability are crucial.

# Read-only domain controller

In Windows Server 2008, the **read-only domain controller (RODC)** concept was introduced as a deployment option. An RODC server holds a copy of the AD database and supports one-way, incoming replication. No direct changes can be made to the directory (such as LDAP writes); the only changes that can occur are those that are made via replication. A change should be made on a DC, allowing replication to push those changes to the RODC.

The RODC role was intended as a branch office for other locations that may not have adequate security to protect against Kerberos system components being compromised, such as the **KDC**, which is a database of keys, and the **Ticket Granting Service (TGS)**, which is used to obtain Kerberos session keys. Unlike DCs, which share a Kerberos key, RODCs each have a unique key, minimizing the risk of exposing remote sites that may be isolated from secured and controlled corporate networks.

RODCs have two other use cases. The first is where locations, such as branch offices, don’t have a lot of bandwidth for replication traffic. The other scenario is to support the principle of least privilege so that the local server admin account does not need to have domain admin access to function.

This section looked at DCs and their role in AD DS. Now, let’s look at the global catalog and its purpose.