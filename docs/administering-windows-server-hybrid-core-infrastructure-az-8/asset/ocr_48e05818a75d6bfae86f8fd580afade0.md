These trust relationships are shown in the following diagram:

![](bbox=[0.18, 0.13, 0.78, 0.42])

All automatically created two-way trusts in the forest are transitive. If domain B is trusted by domain A, and domain D is trusted by domain C, then domain A trusts domain C and D. This is a complex way of saying all the domains trust all the other domains in the forest.

However, in contrast, trusts between forests are not transitive, which means that if there are trusts between forests A and B and forests B and C, then C is not implicitly trusted by forest A.

Note that when a trust is set up between forests or domains, a function known as **SID filtering** is enabled by default. The purpose is to remove all foreign **security identifiers (SIDs)** from a user's access token when using a trust in the trusting domain to access resources. Although it should be disabled to allow access to resources via the trust, you can disable this on the outgoing trust of the trusting domain.

When it comes to SID filtering and resource access across trusts, it is important to understand the trust direction and its relationship to the direction of access and which way the trust relationship is set up – that is, outgoing or incoming. This can be unclear; the core concept is that the direction of trust will be the opposite of the direction of access. This is shown in the following diagram: