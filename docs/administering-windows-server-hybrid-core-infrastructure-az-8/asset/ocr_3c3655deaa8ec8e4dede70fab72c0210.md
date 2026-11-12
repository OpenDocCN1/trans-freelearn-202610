In this section, we looked closely at various AD DS components. In the next section, we will look at the function of AD DS trusts.

# Trusts

Trusts are implemented to access resources when multiple domains and forests exist within an environment. Depending on the type of trust, these trusts can be one way or two way.

A trusted domain object is stored in the system container in AD DS and provides information on the trust types that have been created.

The most common trust types that can be implemented are as follows:

+   - **Parent and child trust**: This is created when a new domain is added to an existing tree. It is a transitive, two-way trust.
+   - **Tree-root trust**: This is created when a new tree is added to an existing forest. It is a transitive, two-way trust.
+   - **Forest trust**: This is a trust between forests and allows two forests to share resources. It is a transitive one-way or two-way trust.
+   - **Shortcut trust**: This is a trust created manually when authentication time between domains in different parts of the forest needs to be reduced. It is a nontransitive one-way or two-way trust.
+   - **External trust**: This is a trust that allows access to resources from a domain in another forest or an NT 4.0 domain. It is a nontransitive one-way or two-way trust.
+   - **Realm trust**: This is a trust between AD DS and a directory service other than AD DS that implements a Kerberos version 5 protocol realm. It is a transitive or nontransitive one-way or two-way trust.