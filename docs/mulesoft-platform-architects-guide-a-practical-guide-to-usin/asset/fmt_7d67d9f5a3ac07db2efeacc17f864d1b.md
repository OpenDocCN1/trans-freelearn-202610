
## Improved Collaboration Across Silos

The most powerful component of an application may end up being improved collaboration across previously siloed departments, teams, or groups within an enterprise. As the application network grows, we begin to securely unlock data across the enterprise. Without a concerted effort to evangelize these new capabilities, we may end up losing some of the primary benefits behind the application network. As a practical approach to this best practice, the platform architect should encourage open communication. While tools can support much of what we do in integration, the best ideas and innovations come from individuals who understand what services are now available. This communication should be done across functions where possible as well.

## Design Your APIs with the Consumer in Mind

Your APIs should be easy to understand, use, and integrate with other applications. You should also follow the principles of `REST` (`representational state transfer`), including resources, methods, status codes, and hypermedia links. With this best practice comes the challenge of finding the balance of functionality and features and what granularity the API should be at.

### Document Your APIs with Clear Specifications

Your APIs should have clear and comprehensive documentation that describes their functionality, parameters, responses, errors, and examples. You should also use a standard format to document your APIs, such as `OpenAPI Specification` (`OAS`) or `RESTful API Modeling Language` (`RAML`), which is a widely adopted standard for describing RESTful APIs.

Beyond these specifications, it is critical to provide good developer documentation within Exchange. Stripe and Fiserv’s API documentation are often considered the gold standard, and each provides clear examples of how to use their services. We should not be surprised when an API in Exchange is left unused and a new API is developed, as the Exchange entry held nothing more than the resources (functionality) and the operations (`GET` and `PUT`). (See the earlier Innovation and collaboration section.)

## Managing Your APIs with Anypoint API Management

While MuleSoft loves to imagine all the APIs within an organization as Mule APIs, the reality is that organizations very often have APIs developed using many different tools. Being able to manage these APIs in a single place also gives us the ability to gain visibility of a full application network, and visibility is one of the key benefits of an application network.

## Governing Your APIs with Policies and Standards

With `Anypoint API Governance`, we can create policies and standards defining how APIs on the application network should be designed, developed, tested, deployed, and maintained. The platform allows us to ensure compliance with these policies and standards, as well as with regulatory requirements and best practices.

## Evolving Your APIs with Feedback and Iteration

Your APIs should be constantly improved based on the feedback from your consumers, partners, and stakeholders. You should also adopt an iterative approach to developing your APIs, using agile methodologies such as `Scrum` or `Kanban`. This will help you deliver value faster and more frequently, as well as adapt to changing needs and expectations.

## Summary

In this chapter, we started learning how to view the problem of integration using a different paradigm – a viewpoint that sees applications and systems across an enterprise, both internal and external, as nodes of a network. We learned that this approach to architecting enterprise integration is something `MuleSoft` calls an application network. While this approach is not exclusively a `MuleSoft` idea, the platform presents the architect with the tools, governance, security, and execution needed to enable the application network.

In addition to examining the logical concepts of the application network, we also took some time to examine the physical manifestation of this architecture in the Building and implementing an application network section of this chapter. We presented the characteristics of an application network and studied some of the benefits and challenges of architecting and designing these networks. We also looked at what is meant by a composable enterprise and the impact this can have on an IT organization.

Finally, we identified best practices that the platform architect should consider when designing, building, and implementing an application network.

## Questions

1. In the context of `MuleSoft Anypoint Platform`, what are two key components that help form an application network?

2. What is the importance of developing an architecture strategy for an application network?

3. How will a company benefit from developing and/or exposing an application network within the enterprise?

## Further Reading

- *The New IT Operating Model – Application Networks, Component Exchanges, and `Federated Enablement`*
- *Catalyst: Projects Delivery – Manage the Solution: Build `Solution Roadmap`*
- *Why `Mule` is not your data `strategy`* ([https://blogs.mulesoft.com/learn-apis/application-network/where-mule-fits-in-your-data-strategy/](https://blogs.mulesoft.com/learn-apis/application-network/where-mule-fits-in-your-data-strategy/))

## Answers

1. Two key components of an application network are a system of record applications and external, or consuming applications. The application network acts as the internal nervous system, connecting these different applications with system APIs that securely expose just the right amount of data to process and experience APIs that combine, orchestrate, choreograph, and present this data to approved and authorized consumers.

2. Developing a roadmap allows the platform architect to visually identify APIs that can be reused, based on new consuming applications as they show up on the roadmap.

3. The application network can improve the efficiency of the business as well as the efficiency of the IT team. The application network can create better scalability through reuse and improve the security of the data by locking it down to a single exposed interface, controlled through various authentication and authorization capabilities. Finally, the application network improves the visibility of the flow of data and the reuse of the building blocks that form the network.

# Speeding with Accelerators

In this chapter, we will examine `artifacts`, `pre-built frameworks`, and `APIs` that are directly available to platform architects and developers from `MuleSoft` and can be found in `Anypoint Exchange`. Collectively, these components are referred to as `accelerators`. These `accelerators` can expedite the progress of a project or an organization in terms of architecture and development. They also highlight and exhibit numerous standards and best practices, which can be studied academically for instructional purposes. For organizations that are just beginning to address their API landscape, these standards and best practices may be sufficient to enhance or enrich existing (or missing) company standards and best practices.

## Topics Covered

- Seeing what `accelerators` `MuleSoft` has developed and included in Exchange
- Understanding how using an `accelerator` can get your organization up and running faster than by starting with nothing
- Learning what building blocks make up these `accelerators`
- Thinking critically about some of the limitations of the `accelerators`
- Briefly reviewing additional acceleration artifacts found in `Catalyst`’s `GitHub` repository
