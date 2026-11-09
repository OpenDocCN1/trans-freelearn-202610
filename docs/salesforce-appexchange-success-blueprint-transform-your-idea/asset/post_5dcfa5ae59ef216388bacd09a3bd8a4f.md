
## Further reading

If you’re keen on delving further into the technical nuances of managed packages, I’d suggest checking out the book *Salesforce Platform Enterprise Architecture*, authored by Andrew Fawcett. This resource can aid in cultivating a more profound comprehension of the capabilities offered by the Salesforce platform and in uncovering effective ways to leverage them for application development.

## Seamless Integration with External Systems

![](img/44ed632038957e4c80220676e84c5109_241_0.png)

Welcome, fellow trailblazers, to our next adventure: the exciting terrain of seamless integration with external systems in Salesforce! If our journey thus far has been about understanding the lay of the Salesforce land, this chapter will teach us how to build bridges that connect our creations to the world beyond.

What you’re about to learn in this chapter isn’t just an essential skill set; it’s the cornerstone of crafting robust applications that can nimbly cross the chasms between disparate systems. In the vibrant ecosystem of Salesforce, each element you master is another tool in your adventurer’s kit. So, let’s review the tools we’ll be adding in this chapter:

- Technical architecture: Consider this your blueprints for bridges of different types. We’ll explore the two main blueprints available for ISVs and how they influence the bridges you build.
- Integration patterns: These patterns are like your trusted guidebooks. Just like cultivating a vibrant garden requires understanding the unique needs of different plants, integrating your AppExchange application with other systems requires understanding different integration patterns.
- Salesforce APIs: APIs are like the materials you’ll need to build your bridges, enabling a connection between your application and external platforms.
- Salesforce Connect: Imagine this feature as a magic carpet that can fly data from distant lands (external sources) directly into your Salesforce instance.
- Canvas SDK: This toolkit is like a team of skilled artisans, helping you embed third-party web applications within the Salesforce interface.
- Connected apps: These custom-built applications are like secure passes, granting access across the borders between Salesforce and external systems.

By the time we finish this chapter, you’ll be skilled at architecting bridges between your applications and external systems, using Salesforce’s exceptional tools. Pack your gear, fellow adventurers, as we continue our thrilling exploration into the Salesforce wilderness! Onwards to greater understanding, integration, and, yes, adventure!

## Technical architecture

Embarking on the journey of building an application in collaboration with Salesforce can feel a bit like standing at the crossroads of a grand metropolis. You have two main avenues to start from, each leading to a unique type of application.

The first avenue leads to native apps, which are primarily built within the Salesforce ecosystem and do not integrate with external systems. The second is the route to composite apps, which are designed to integrate seamlessly with other systems beyond Salesforce.

These avenues are distinguished by the amount of Salesforce technology employed in the construction of the application. However, it’s worth noting that these paths aren’t strictly linear. Sometimes, partners find themselves transitioning from native to composite applications and vice versa, showing that these avenues may intersect and cross over time.

Just as a city’s architecture can give it a unique flavor, the choice of application type deeply influences its design. Each application is crafted to meld seamlessly into the customer’s Salesforce landscape, just like a new building blending harmoniously into a city skyline. But here comes the pivotal part. This choice isn’t simply about aesthetics or functionality within the Salesforce ecosystem. For composite apps, it’s paramount to consider the integration with external systems, which might necessitate the use of technologies and concepts beyond what was covered in the previous chapter.

Much like a city planner considering road connections while designing a new district, your decision on the type of application influences the integration pathways with external systems. It’s not merely about constructing a standalone entity but rather a connected, interactive component of a larger network. Therefore, as you start sketching your application blueprint, remember this: the type of application you’re building defines not only its standalone structure but also the way it interacts and integrates with, and potentially introduces complexities into, the world beyond Salesforce.

## Native applications

Native applications are built entirely on the Salesforce platform and take full advantage of Salesforce’s infrastructure management, core capabilities, and innovations. This type of application benefits from the most robust features available on the market, as they are built 100% on top of the Salesforce platform.

Key aspects of native applications include the following:

- Built entirely on the Salesforce platform
- Benefit from Salesforce’s infrastructure management and core capabilities
- Seamless integration into a customer’s Salesforce instance
- Rapid development and innovation
- No worries about data residency – all the data is hosted right where your customer’s Salesforce instance is located

## Composite applications

Composite applications are the most common type of application built by ISVs. These applications are predominantly built off-platform but incorporate some functionality built on the Salesforce platform. By leveraging Salesforce technology, composite applications can fit more seamlessly into a customer’s Salesforce instance, driving greater visibility and higher adoption rates.

Key aspects of composite applications include the following:

- Parts of the application are built off-platform
- Some functionality is built on the Salesforce platform
- Seamless integration into a customer’s Salesforce instance
- Greater visibility and higher adoption rates

## Extension packages for integrations

When integrating Salesforce with external systems, it’s essential to ensure that the core functionalities of your product remain unaffected. If integration isn’t a central component of your core product, consider delivering it as an extension package, a concept you might recall from *Chapter 4*.

The idea of keeping integrations outside the core package stems from the need for decoupling. This separation ensures that any potential issues or changes in the integration module don’t impact the core product’s performance or functionality.

Furthermore, customers often have diverse integration needs. Some might want to connect with a commonly used external system, while others might have specific requirements tailored to their operations. By employing extension packages, you can cater to these varied needs without complicating your core product.

In essence, while integrations play a crucial role in enhancing your product’s capabilities, it’s vital to approach them with a modular mindset. Extension packages offer a structured solution, ensuring a streamlined core product while providing the flexibility to address diverse integration requirements.

## Integration patterns

![](img/44ed632038957e4c80220676e84c5109_247_0.png)

Creating an AppExchange application is akin to cultivating a beautiful garden. Each integration pattern is a different plant or flower that contributes to the richness and variety of the garden. Some plants may need more water (data), others more sunlight (APIs), and still others more nutrients (user interactions). It’s all about understanding their unique needs and nurturing them accordingly for your garden (application) to flourish. Salesforce has made remarkable strides in defining these integration patterns, which are covered in one of their esteemed certifications: Integration Architect¹. The Integration Architect certification equips professionals with the knowledge and expertise needed to effectively navigate the intricacies of application integration. For organizations heavily reliant on integrations, it is highly recommended to ensure that at least one individual in the technical team has obtained this credential. By doing so, they can leverage the insights gained from Salesforce’s comprehensive integration pattern framework, thereby optimizing their integration processes and enhancing overall application performance.

1 https://trailhead.salesforce.com/help?article=Salesforce-Certified-Integration-Architect-Exam-Guide

## Remote process invocation—request and reply

This pattern involves a client application making a request to a remote service and waiting for a response. The client is blocked until the response is received or a timeout occurs. This pattern is commonly used when the client needs to process the response before it can proceed.

For example, an ISV might use this pattern to integrate a third-party payment processing service into their AppExchange application. When a customer makes a payment, the application sends a request to the payment service and waits for a confirmation response before proceeding.

## Remote process invocation—fire and forget

In this pattern, the client application sends a request to a remote service but does not wait for a response. This is useful when the client does not need to
