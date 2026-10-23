+   - Organized around business capabilities
+   - Owned by a dedicated team

Independent deployability is of crucial importance. You should be able to deploy new or updated services into production without being concerned about disrupting other services or having to remember to redeploy these other services. This will be described in more detail in the *Introducing the 12-factor app* section of this chapter.

Loose coupling in software development is always a good idea, but even more so in microservices. It allows for services to change over time without these changes affecting other services. There are many things to consider in order to achieve loose coupling, but the most important ones are using distinct database schemas for each of your services, communicating via APIs (remember the quote from Jeff Bezos of Amazon on this: “*All service interfaces, without exception, must be designed from the ground up to be externalizable*”), and providing asynchronous communication between your services.

That these services should be organized around a business domain goes without saying: their sole reason for existence is that they in some way support the business process of the company that is utilizing them.

Finally, they should be owned by a small team. The reason for this is that these services only facilitate a small part of the complete business process. As such, the amount of domain logic that is required is also limited. Compare this to traditional application development where every developer had to potentially work on the entire application. This would require knowledge of the complete business domain.

Onboarding new members into your team becomes much easier and quicker with microservices. Also, changes to your microservices can be implemented much quicker as each developer knows all the details and the effect that these changes will have.

# Containers and orchestration

In the chapters on Docker (*Chapter 6*) and Kubernetes (*Chapter 7*), we have already explained the advantages of using these technologies. In fact, you might argue that containerization is what made the cloud-native movement possible in the first place.

Creating artifacts, your application binaries, in an unmodifiable container that is pushed through the different stages of a pipeline (development, staging, production) has enormous advantages.

In systems consisting of dozens, hundreds, or even thousands of containers, some kind of container orchestration tool is a necessity. It also means that part of your operations will be automated, relieving the operations department of trivial work and allowing them to focus on more important and interesting tasks at hand.