The following diagram illustrates different autonomous systems; some have relationships that are bi-directional and some have a one-way relationship or dependency:

![](bbox=[0.3, 0.16, 0.7, 0.4])

Figure 2.1 – Complex adaptive systems' relationships

In this section, we explored complex adaptive systems. We can think of microservices as being very similar in nature to a complex adaptive system. We now need to determine whether the microservice we are creating is complex or not, as this will help determine our approach for building the microservice in question.

# Complex or not complex – that is the question

Microservices can be simple, complicated, or complex, and if we do not understand them, we can fall into chaos. So, we can then use a process—or rather, a framework— to help us to decide whether we are dealing with a simple, complicated, or complex situation in order to determine what type of microservice we are building and what level of effort and resources we will need to build that service, be it people, time, or effort. We can determine whether we can buy something off the shelf for simple situations, or we can implement an existing framework. We can also determine whether we need to involve an expert to tell us which frameworks and tools we need and how to configure such tools. Finally, we can determine whether we are dealing with a complex problem and, if so, we need to assign the best people, resources, and efforts to build our microservice.

The following diagram illustrates that different microservices can address a range of complexity, from simple services such as a catalog lookup to services that are complicated, such as account services or invoicing services, and some services can be complex as they are responsible for implementing business logic or the routing of shipments or goods. We can see that each service can have a dependency on each other, and we now need to determine what level of complexity we are dealing with: