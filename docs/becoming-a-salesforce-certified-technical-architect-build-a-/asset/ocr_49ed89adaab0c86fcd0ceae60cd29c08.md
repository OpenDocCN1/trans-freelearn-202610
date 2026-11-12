+   5. The server validates the client’s certificate with the mentioned CA (verifying digital signature). Usually, the server will be configured to restrict access to specific clients only. In this case, the server will compare the given certificate to a repository of *permitted client certificates* that it holds. If the given certificate is among them, it will proceed to the next step.

+   6. The rest of the steps are the same as one-way TLS.

Two-way TLS is a very popular mechanism particularly in the SaaS world because many production environments (in the Salesforce world, these are known as *production instances*) share the same infrastructure, domain name, and IP range. Two-way TLS provides a convenient way to ensure that the right instance can be accessed by the right set of client applications. Now, let's look at a quick example to explain this further.

# Putting Both Integration and Cryptography Together

Assume you have a Salesforce instance that has been integrated with a set of cloud-based and on-premises-hosted applications through an ESB middleware such as *MuleSoft CloudHub*, which is a cloud-based, multi-tenant, and fully managed platform. You need to ensure that only your enterprise’s Salesforce instance can communicate with your enterprise’s MuleSoft CloudHub instance and vice versa. Two-way TLS would provide that functionality and would ensure your data is secure and always encrypted in transit. However, in the next chapter (*Chapter 4, Core Architectural Concepts: Identity and Access Management*), you are going to learn that you might need to add another authentication layer to that architecture for some use cases.

# Summary

In this chapter, you started by looking at the general concepts surrounding integration before moving on and looking at some of the old and modern integration approaches. You achieved this by looking at different types of tools and mechanisms. You also learned about some ideal use cases for each of these tools.

You then moved on to a related architectural topic, that is, security and cryptography. You learned about some key concepts in the wide and complex world of cryptography and tied them back to some real-world examples that you may come across regularly. Finally, you tied all this together and learned how integration and cryptography can work side by side in the enterprise landscape.

In the next chapter, you will cover another key architectural concept. IAM architectural concepts are now extensively used in modern distributed solutions. These IAM concepts are closely related to both the integration and encryption domains, and they are common in cloud-based solutions such as Salesforce. While looking at these IAM concepts, you will look at their definitions, standards, and flows.