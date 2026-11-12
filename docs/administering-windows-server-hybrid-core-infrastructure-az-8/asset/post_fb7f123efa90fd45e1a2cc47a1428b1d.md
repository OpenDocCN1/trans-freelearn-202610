
When the AD DS role is installed on Windows Server, a deployment option is provided for creating a new domain in an existing forest or a new domain in a new forest. The first domain in a new forest becomes what is known as the *forest root domain*; this is analogous to saying whether you would like to create a city in an existing state or whether you would like to create a new state and make this the first and parent/founding/capital city in that state – that is, the first domain in the new forest becomes the capital city, also known as the forest root domain.

This section looked at the AD DS domain and domain tree topology. The next section will look at the AD DS forest topology.

## Forests

A **forest** is a top-level logical container definition. It is a collection of one or more domains that share a *common parent root domain*, *schema*, and *global catalog* that has a contiguous namespace; it is considered a *security* and *administrative* boundary.

To implement a security boundary for objects, they must be placed in different forests; all objects in a forest share the same security controls. It is a common misunderstanding that placing objects in different domains will isolate them between domains. It is essential to note this is not the case; only placing them in different forests can provide this isolation. The following diagram shows a visual representation of a forest:

![](img/cf35f46a8dd5f11e58349c1d81611b75_37_0.png)

The forest topology (structure or layout) means that a forest can contain one domain or multiple domains across multiple domain trees, much like a country can contain many states, which are collections of independent and autonomous cities. Each state would contain a state capital city; we refer to this as the forest root domain.

In this section, we looked closely at various AD DS components. In the next section, we will look at the function of AD DS trusts.

## Trusts

Trusts are implemented to access resources when multiple domains and forests exist within an environment. Depending on the type of trust, these trusts can be one way or two way.

A trusted domain object is stored in the system container in AD DS and provides information on the trust types that have been created.

The most common trust types that can be implemented are as follows:

- **Parent and child trust**: This is created when a new domain is added to an existing tree. It is a transitive, two-way trust.
- **Tree-root trust**: This is created when a new tree is added to an existing forest. It is a transitive, two-way trust.
- **Forest trust**: This is a trust between forests and allows two forests to share resources. It is a transitive one-way or two-way trust.
- **Shortcut trust**: This is a trust created manually when authentication time between domains in different parts of the forest needs to be reduced. It is a nontransitive one-way or two-way trust.
- **External trust**: This is a trust that allows access to resources from a domain in another forest or an NT 4.0 domain. It is a nontransitive one-way or two-way trust.
- **Realm trust**: This is a trust between AD DS and a directory service other than AD DS that implements a Kerberos version 5 protocol realm. It is a transitive or nontransitive one-way or two-way trust.

These trust relationships are shown in the following diagram:

![](img/cf35f46a8dd5f11e58349c1d81611b75_39_0.png)

All automatically created two-way trusts in the forest are transitive. If domain B is trusted by domain A, and domain D is trusted by domain C, then domain A trusts domain C and D. This is a complex way of saying all the domains trust all the other domains in the forest.

However, in contrast, trusts between forests are not transitive, which means that if there are trusts between forests A and B and forests B and C, then C is not implicitly trusted by forest A.

Note that when a trust is set up between forests or domains, a function known as **SID filtering** is enabled by default. The purpose is to remove all foreign **security identifiers (SIDs)** from a user's access token when using a trust in the trusting domain to access resources. Although it should be disabled to allow access to resources via the trust, you can disable this on the outgoing trust of the trusting domain.

When it comes to SID filtering and resource access across trusts, it is important to understand the trust direction and its relationship to the direction of access and which way the trust relationship is set up – that is, outgoing or incoming. This can be unclear; the core concept is that the direction of trust will be the opposite of the direction of access. This is shown in the following diagram:

![](img/cf35f46a8dd5f11e58349c1d81611b75_40_0.png)

Figure 1.8 – Trust direction

This means that if you want to access resources in another forest, there must be an outgoing trust; conversely, an incoming trust allows outgoing access.

One final concept to note is that of the trust authentication types. External and forest trusts provide two modes of authentication: forest-wide authentication and selective authentication.

Further information about trusts can be found at https://docs.microsoft.com/en-us/azure/active-directory-domain-services/concepts-forest-trust.

In this section, we looked at AD DS trusts. Now, let’s look at the AD DS data store.

## Data store

A directory information store holds the AD database, which uses the Extensible Storage Engine (ESE) based on the Jet database engine.

The database is made up of two files – the database file itself (named NTDS.DIT and located in C:\Windows\NTDS directory by default) and the transaction log file (named EDB.LOG and also located in C:\Windows\NTDS directory by default). These files are stored on each DC. The group policy files for the group policy objects are stored in the SYSVOL folder.

The AD DS database file (NTDS.DIT) contains four partitions (or naming contexts) that contain all the domain-related information. Different data is stored in each partition, and each partition is replicated between DCs with a dependent replication scope and schedule.

These partitions are shown in the following diagram:

![](img/cf35f46a8dd5f11e58349c1d81611b75_41_0.png)

Figure 1.9 – AD DS database partitions

Let's look at these four directory partitions in more detail:

- The **schema partition** is where the AD DS schema is stored; this defines what can be stored in the AD database
- The **configuration partition** is where the configuration information for the forest and domain trees is stored, such as site and replication information
- The **domain partition** is where object information for the domain is stored – that is, information about the user and computer objects
- The **application partition** is where applications can store data in AD; multiple application partitions may exist

In this section, we looked at the AD DS data store, which contains the AD DS database. We also looked at the partitions for the AD database. In the next section, we will look at the AD DS schema.

## Schema

The **schema** is a partition of the AD database; it provides the definitions for the object classes (categories) that can be stored in the directory and the object-describing attributes. The object classes and object attributes are schema objects; in other technology areas, they would be referred to as metadata – that is, data describing the data.

The schema can be edited and allows you to create, modify, and disable object categories and attributes of objects. The schema is forest-wide, so any change is replicated to every domain in the forest. Understand the impact before committing any changes to the schema. In addition, be aware that deletions are not supported in the schema.

The default built-in group schema administrators control access to the schema; you must be a member of this group to make changes to the schema.

In this section, we looked at the AD schema. In the next section, we will look at AD DS DCs.

## Domain controllers

A DC is a server with the AD DS role installed; it holds a writeable copy of the AD database and functions through the multi-master replication model for this information store. It also provides a store for GPOs. It ensures that the configuration settings are applied to domain-managed objects.

The DC is also a Kerberos **Key Distribution Center (KDC)**. Kerberos is the mechanism for all authentication and authorization within AD; a Kerberos key identifies an object (authentication – that is, who you are) and outlines what resources an object can access (authorization – that is, what you can do and access).

DCs must be highly available to process logon requests; they are required to authenticate requests for access to domain resources and apply policies for managing and configuring objects in the directory stored in the AD database.

All computers and users that wish to access objects in an AD DS domain must be authenticated by a DC, so their placement, operation, and availability are crucial.

## Read-only domain controller

In Windows Server 2008, the **read-only domain controller (RODC)** concept was introduced as a deployment option. An RODC server holds a copy of the AD database and supports one-way, incoming replication. No direct changes can be made to the directory (such as LDAP writes); the only changes that can occur are those that are made via replication. A change should be made on a DC, allowing replication to push those changes to the RODC.

The RODC role was intended as a branch office for other locations that may not have adequate security to protect against Kerberos system components being compromised, such as the **KDC**, which is a database of keys, and the **Ticket Granting Service (TGS)**, which is used to obtain Kerberos session keys. Unlike DCs, which share a Kerberos key, RODCs each have a unique key, minimizing the risk of exposing remote sites that may be isolated from secured and controlled corporate networks.

RODCs have two other use cases. The first is where locations, such as branch offices, don’t have a lot of bandwidth for replication traffic. The other scenario is to support the principle of least privilege so that the local server admin account does not need to have domain admin access to function.

This section looked at DCs and their role in AD DS. Now, let’s look at the global catalog and its purpose.
