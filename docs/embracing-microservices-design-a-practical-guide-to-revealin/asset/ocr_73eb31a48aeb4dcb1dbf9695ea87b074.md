The following figure illustrates the contract testing steps and the interactions between the consumer and the provider:

![](bbox=[0.142, 0.148, 0.824, 0.522])

Figure 9.3 – Contract testing

Contacts are for both consumers and providers, which ensures that they are in agreement on the API level. Each contract's interactions with consumers or providers are tested in different ways:

+   - Consumer tests use the contracts to configure stubs (returns value) that simulate the provider, enabling you to write integration tests for a consumer that does not require a running provider.
+   - Provider tests use contracts to test the SUT, using mocks for SUT dependencies.
+   - Contract testing provides a mechanism for mocking and stubbing or integrations. We can use Pact.io to provide the capability of testing integrations between services and repositories. Contract tests can provide a test for HTTP requests or messaging approaches and are called pacts:
+     1. **HTTP pact:** An expected request is what the consumer is expected to request. This in turn has a minimum expected response, which is the parts of the response the consumer wants the provider to return.