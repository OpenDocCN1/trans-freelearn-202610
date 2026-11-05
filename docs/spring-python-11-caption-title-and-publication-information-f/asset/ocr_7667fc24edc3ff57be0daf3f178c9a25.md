+   - In our more complex configuration we introduced `PerformanceInterceptor`, where we observed which methods are taking the longest and evaluate them for caching. This was relatively easy to add, because it required no interference with the other interceptors or their pointcuts. This suggests that future updates should be relatively easy to make

With advice decoupled from pointcuts, it is easy to add new pointcuts and new advice, thereby updating our crosscutting behavior. And all of this can be done without impacting the core API we are enhancing.

We have shown that AOP supports loosely coupling solutions. We have also looked at how it is easy to easily apply advice with precision, offering high cohesion. These factors will help us by making it easy to respond to changes we know are coming.

The following diagram shows a high-level view of how the components are wired together.

![](bbox=[0.25, 0.41, 0.75, 0.66])

Each interceptor has its code is cleanly wrapped by an advisor with its pointcuts, forming well defined aspects. Each aspect is also separated from the business logic of the `WikiService`. With this separation of concerns, it is easy to make changes with minimal risk. It is also easy to add new advisors and change the order of execution.