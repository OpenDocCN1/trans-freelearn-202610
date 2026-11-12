attention of the judges to it but then leave it open, allowing them to ask questions about it during the Q&A.

When you leave some topics open because you are not aware of their importance or do not know the answer, this is more of an attempt to blow a smokescreen and hope that the judges will not notice. This is a strategy that you should avoid at all costs.

When you are proposing a Single Sign-On strategy, you need to be crisp and clear about all of its areas unless you are intentionally leaving them open for further conversations. In this scenario, you have to explain your identity management strategy. Your proposed solution could be as follows:

I am assuming that both the ERP and the inventory management systems support the OpenID Connect standard. I propose introducing an identity provider and identity management tool such as Ping Identity to the landscape. Ping will utilize its identity store (PingDirectory) to maintain user credentials.

Currently, user credentials are hosted locally by ERP and inventory management. I propose migrating the user details from these systems to Ping.

I am assuming that it would not be possible to migrate the systems’ passwords because only their hashed values were stored. Therefore, we need to send emails to all the migrated customers asking them to set a new password. The email will contain a link with a protected one-time link that allows them to set a new password using a Ping-hosted page.

Ping will handle provisioning and de-provisioning users to all linked SPs. I will utilize a federated ID to uniquely identify the customer across the systems. This could be the employee ID.

When the user is attempting to access the Salesforce instance using the unique MyDomain URL, the system will detect the authentication providers configured for that instance. And if Ping is the only authentication provider, the user will be automatically redirected to Ping’s login page. The authentication will take place using the OpenID Connect web server flow.

Should you stop there or carry on explaining exactly what that flow looks like? It is up to your strategy and how you are planning to utilize the presentation time. If you stopped there, you are effectively seeding a question. If the judges are interested in testing your knowledge about that flow, they will come back to it during the Q&A. If they are not (because they believe you already know what it takes to pass the IAM domain), then they will not.

This is the power of that technique. If used wisely, you can avoid wasting time explaining areas that you do not necessarily need to.

Are you surprised to see an IAM requirement in a mini scenario focusing on integration? You should not be by now. You learned that the domains are not isolated and are very much related to each other. Now, update your landscape architecture diagram, and move on to the next requirement.