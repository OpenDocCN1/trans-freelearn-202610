
# Practice Questions

## Question 5
An organization has assets in Anypoint Exchange. Some of the versions of those assets are of no use as new versions have been published in Anypoint Exchange. What should happen to the old asset versions so that users are aware that they are not in use?

1. Decommission the asset version
2. Retire the asset version
3. Deprecate the asset version
4. Remove the asset version

## Question 6
How does a fine-grained microservices architecture benefit the application network?

1. The number of discoverable and reusable assets will increase in the application networks
2. The number of API connections will decrease in the application networks
3. Resource usage will decrease as fine-grained microservices require fewer resources
4. Response time will improve for end consumers as microservices are smaller

## Question 7
An organization is looking to set up Anypoint Virtual Private Cloud. To do that, they need to decide on the CIDR mask. They have the following requirements:

* 100 applications are to be deployed on the production
* Each application will be deployed to two workers for high availability

What will be a suitable CIDR mask for these requirements?

1. `0/24`
2. `0/23`
3. `0/22`
4. `0/21`

## Question 1
What cannot be effectively enforced with Anypoint Security?

1. Guarding the API against denial-of-service attacks
2. Limiting the size of the payload
3. Validating the OAuth token
4. Allowing a request from certain IP addresses or IP ranges

## Question 2
An organization has implemented a dedicated load balancer and they want to allow public traffic from certain IP addresses and need to restrict all other traffic on the dedicated load balancer. What is the best solution to implement this requirement?

1. Apply the IP blocklist policy on the API instance
2. Remove `0.0.0.0/0` from allowed CIDRs and add the required IP addresses or IP ranges to the allowlist CIDRs on the dedicated load balancer to allow traffic from those IP addresses and restrict all other traffic
3. Apply the IP allowlist policy on the API instance
4. Apply the IP allowlist policy on the API security to allow traffic from certain IP addresses or IP ranges

## Question 3
An organization hosts APIs on the customer-hosted Mule Runtime, and they have configured Active Directory as an identity provider in the Anypoint Platform. They don't have the budget to go for any other external identity provider. Which API policy can be used to authenticate users within the organization when accessing the APIs?

1. Basic authentication – LDAP
2. Basic authentication
3. JWT validation
4. IP allowlist

## Question 4
An organization has permanently moved API implementation to a new location. What is the best approach to tell the client that the requested resources have been permanently moved to a new location?

1. Respond with status code 500 and the message "Error while processing request"
2. Respond with status code 404 and the message "Requested resource not found"
3. Respond with status code 301 and the message "Requested resource has been moved permanently to a new location"
4. Respond with status code 302 and the message "Requested resource has been moved permanently to a new location"

## Question 5
An organization has an API that connects to the backend system, which intermittently goes down or is under maintenance two to three times a week. The client is looking to avoid overloading the backend system during maintenance, which is likely to fail all the requests. Which API policy can be used to implement this situation?

1. Canary deployment
2. Spike control
3. Rate limiting
4. Circuit breaker

### Note
The canary deployment and circuit breaker policies are available in the MuleSoft Catalyst GitHub.

### Further Reading
* <https://training.mulesoft.com/certification/architect-platform-level1>
* <https://training.mulesoft.com/course/architecture-application-networks>

### Answers
| Question | Answer |
| --- | --- |
| 1 | A |
| 2 | B |
| 3 | C |
| 4 | B |
| 5 | A |
| 6 | A |
| 7 | B |
| 8 | B |
| 9 | A |
| 10 | D |
| 11 | D |
| 12 | A |
| 13 | C |
| 14 | C |
| 15 | D |
| 16 | C |
| 17 | A |
| 18 | C |
| 19 | A |
| 20 | B |
| 21 | C |
| 22 | B |
| 23 | A |
| 24 | C |
| 25 | D |

# 18. Tackling Tricky Topics

We explored MuleSoft's range of hosting options in earlier chapters, including CloudHub, on-premises, hybrid, Runtime Fabric manager on VMs/bare-metal machines, and Self-Managed Kubernetes.

The following topics will be covered in this chapter:

* Considerations for choosing the right deployment model
* Resilient API techniques
* `Anypoint CLI`
* Idempotent and cache scope
* Core components
* Use cases for implementing the platform architecture

## Considerations for choosing the right deployment model

MuleSoft is a very adaptable platform that offers a variety of deployment choices to host your network of apps. To ensure that your enterprise's digital transition is smooth and error-free, you must assess the best deployment model.

For businesses looking to host their network of apps, MuleSoft provides the following deployment options:

* CloudHub 1.0
* CloudHub 2.0
* On-premises Mule Runtime (standalone and hybrid)
* Runtime Fabric manager on VMs/bare-metal machines, OpenShift, and Self-Managed Kubernetes

Selecting a MuleSoft deployment strategy is based on several considerations, including data security, control over data, scalability, and particular business requirements. The following instances will show you when to choose each model. Table 18.1 illustrates the differences and similarities across deployment choices with respect to platform capabilities, data and metadata storage, and platform maintenance and upgrades.

### Table 18.1: MuleSoft deployment considerations (platform, data and metadata, and upgrade management)

| Factors | Fully cloud-hosted (control plane and runtime plane in the cloud) | Fully customer-hosted (control plane and runtime plane in on-premises or customer-hosted) | Hybrid (control plane in the cloud and runtime plane on-premises) |
| --- | --- | --- | --- |
| Platform | Anypoint Platform (control plane)<br>CloudHub 1.0 and CloudHub 2.0 (runtime plane) | Anypoint Private Cloud Edition (PCE) (control plane)<br>On-premises Mule Runtime (runtime plane) | Anypoint Platform (control plane)<br>On-premises Mule runtime (runtime plane) or Runtime Fabric on Self-Managed Kubernetes, VMs/bare-metal, and OpenShift |
| Data and metadata | Data and metadata will be in the vendor's infrastructure | Data and metadata will be in the customer's infrastructure | Only metadata will be in the vendor's infrastructure |
| Platform and upgrade management | Platform maintenance and upgrades will be managed by the vendor (MuleSoft) | The customer is responsible for upgrades, patches, and platform maintenance for Mule runtime and Anypoint PCE. | The customer is responsible for upgrades, patches, and platform maintenance for Mule runtime and Anypoint Platform, and upgrades will be managed by MuleSoft |

Table 18.2 illustrates the differences and similarities across deployment choices with respect to network connectivity.

### Table 18.2: Network connectivity considerations

| Factors | Fully cloud-hosted (control plane and runtime plane in the cloud) | Fully customer-hosted (control plane and runtime plane in on-premises or customer-hosted) | Hybrid (control plane in the cloud and runtime plane on-premises) |
| --- | --- | --- | --- |
