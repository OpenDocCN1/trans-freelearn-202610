For this example, assume that the applications had the following reliabilities:

+   - A1-R = 0.9
+   - A2-R = 0.8
+   - A3-R = 0.5

Here, the overall solution would have a reliability of 0.9 x 0.8 x 0.5 = 0.36.

For the sake of simplicity, it is assumed that the reliability of each of the given applications does not change with time and does not get impacted by any external factors (for example, a reduced server capacity or network lag). Historical failure rates for each of these applications are also not factored in. Using a hub-and-spoke approach instead of P2P would significantly increase the overall reliability of your solution as it simplifies the connections. The hub itself can turn into a bottleneck, but you have fewer things that could go wrong.

# Limited Capabilities

Since all data transformations and process orchestrations are taking place on each individual channel, many of these channels end up as straightforward integrations. While simplicity is an architectural goal that should be aimed for, the limited technical capabilities imposed by P2P would effectively limit the business's ability to introduce complex business differentiator services.

# Turns IT into a Blocker

Over time, P2P integrations would inevitably become difficult to maintain and support. Teams will worry about attempting to change anything in them, and the process of deploying any new feature or even a hotfix will become increasingly slow and costly. This would effectively turn the IT department into a blocker rather than an enabler for the enterprise.

# High Dependencies

Typically, tightly coupled integrated applications have high dependencies on each other. You came across this earlier, in the context of reliability and cost of ownership. However, it can also be extended to other dimensions.

Consider a use case where three out of 10 of the integrated applications mentioned earlier have reached their end of life and need to be replaced. Due to the high dependencies in a P2P-based landscape, this will likely have a direct impact on all the other applications in the landscape.

# Scalability

Due to all the previous reasons, it is easy to understand why P2P is not the right option to design a scalable enterprise solution.