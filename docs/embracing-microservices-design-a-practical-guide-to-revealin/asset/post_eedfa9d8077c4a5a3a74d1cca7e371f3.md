
Horizontal tests run across multiple microservices so all those microservices need to be up and running at the time the test executes. This is what makes this type of testing complex and why end-to-end testing should be a smaller portion of your overall testing strategy.

Horizontal testing can include external endpoints or microservices out of our control, which leads us to our next topic on types of environments.

## White box and black box environments

In the white box type, testing users can see what is going on, have a UI to interact with and perform actions, and can see whether results have met their expected outcomes.

During white box testing, there are some actions that are not visible to the user, such as calls to backend services that will return the results the user expects. These invisible services are called black box environments.

These backend services are called a black box environment because the actions are said to be performed in the dark, out of the view of the tester or user.

The best practice for horizontal testing is to write a journey test that is related to the journey a user takes through the system instead of writing a test for each operation.

## Journey testing

So, for journey testing, write one test that includes the operations involved. For example, if it's an ordering system, test creating an order, canceling an order, and updating an order, which represents the journey a user takes.

An end-to-end test is an acceptance test and should be treated as such. Best practice is to use Gherkin and some type of framework such as Cucumber or SpecFlow to run your acceptance test.

## Tools

Docker Compose can provide a way to get all the services up and running.

Tools and frameworks can make end-to-end testing easier to manage. Use SpecFlow, Cucumber, or Cascade to automate these tests.

SpecFlow and Cucumber are behavior-driven design frameworks that work with existing testing frameworks such as xUnit, NUnit, and JUnit. They run a test that is based on the Gherkin language.

Gherkin is a human-friendly language and uses a set of special keywords as follows:

- GIVEN is the context or state the system under test is currently in for this scenario.
- WHEN is the action taken by a user.
- THEN is the expected outcome of the action.

These keywords give structure and meaning to an executable specification. They are steps that form a scenario and are translated into step definition that is expression and combined with a test method. Consider the following example of Cucumber framework step definition:

- Scenario: Products selected
- Given: I have 3 products

Considering the given information, the step definition with an expression and a supporting Java method is as follows:

```
@Given("I have {int} products")
public void i_have_products(int int1) { }
```

With this, we conclude our discussion of end-to-end testing. Another type of testing that is similar but more focused on a smaller scope than end-to-end testing is integration testing. It is deployed for testing the interaction between two separate microservices.

## Integration testing

A microservice needs to interact with other microservices, so testing these interactions is vital but presents some challenges. With integration testing, the primary challenge is that we need to test-run the instances of all the involved services (for instance, when testing a Web API that interacts with a database). This creates the need for complex setup and maintenance of these tests. In the next section, we will explore strategies to address these challenges.

## Strategy

Testing interactions between microservices is vital. Integration testing of microservices presents some challenges. We will explore some strategies you can employ. We need to test things such as gateways and the consumption of RESTful API endpoints and domain event messaging being properly consumed and whether the data and the state produced by these services are properly persisted into a database. Unlike end-to-end testing, we do not need all the services to be running; we just focus on a smaller set of interactions, such as the interaction between a repository class and an order class, testing to make sure the test order was persisted properly.

Microservices often need to persist data in a database-like aggregates such as an order, in a document, or in a relational database of some type.

We also have a service that employs patterns such as CQRS that need to read and write data many times, tested by using in-memory objects.

Docker can be used to containerize the database, which can provide the integration database for testing. While you can use this and mocking methods to test integration, they can be difficult to manage and maintain. This brings us to the best practice of using the concept of contracts for testing the integration of microservices. Let's look at contracts and how they can help us test microservice integration.

## Contract testing

A **contract** is a concrete example of an interaction between two services. The way we build these contracts depends on the type of interaction involved. Contract testing has two types of contracts: consumer and provider, or producer.

The following figure illustrates the contract testing steps and the interactions between the consumer and the provider:

![](img/e6276d4d880001c04e81cb4db4898f1c_264_0.png)

Figure 9.3 – Contract testing

Contracts are for both consumers and providers, which ensures that they are in agreement on the API level. Each contract's interactions with consumers or providers are tested in different ways:

- Consumer tests use the contracts to configure stubs (returns value) that simulate the provider, enabling you to write integration tests for a consumer that does not require a running provider.
- Provider tests use contracts to test the SUT, using mocks for SUT dependencies.
- Contract testing provides a mechanism for mocking and stubbing or integrations. We can use Pact.io to provide the capability of testing integrations between services and repositories. Contract tests can provide a test for HTTP requests or messaging approaches and are called pacts:
  1. **HTTP pact:** An expected request is what the consumer is expected to request. This in turn has a minimum expected response, which is the parts of the response the consumer wants the provider to return.
  2. **Messaging pact:** Contracts support modern distributed systems that use messaging frameworks such as RabbitMQ, Kafka, or Azure Service Bus. These types of interactions are called "message pacts." This is achieved by abstracting the frameworks out and just focusing on the message being passed between.

## Tools

Contract testing is the best practice for microservices integration testing and you can use frameworks such as **Pact.io** or **pactflow.io** to create a contract file that is a concrete JSON document that provides the specification of the provider and the consumer.

Pact is good for designing and testing integrations where teams in your organization control the development of the consumers and the providers, and also where the consumer's team will drive the provider's team requirements or features. So, in short, Pact is best for white box integrations where the teams have a good understanding of the system under test.

I encourage teams to explore contract testing for integration. To really explore contract testing, go to Pact.io for more information on Pact.io and Pactflow.io.

We have explored integration testing and some best practices around it. Now we need to ensure that our application can meet the demands of our users and provide a good user experience.

## Load testing

Load testing can help identify issues with horizontal and vertical scaling bottlenecks and issues with threading or connection pools, to name a few. It can inform us on how we need to scale our applications or help identify any blocking calls for async operations. Load testing can verify latency as requests and responses flow through the system network.

Microservices may have a dependency on other services or resources and these services can have varying degrees of performance. Some may be fast and sometimes some services can have an issue and run slower than expected. We need to know how the system will react to these varying degrees of performance and how it will impact the service we are testing.

The microservice environment is dynamic and constantly changing. We need to test our services to see whether they can handle this dynamic nature of microservices, and how all of this will handle the load of active users in a production environment.
