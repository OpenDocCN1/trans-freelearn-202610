We have seen ways to avoid retrieving unnecessary data from the data store. In the next section, we will explore the `N + 1 Select` problem.

# # The N+1 Select problem

The `N + 1 Select` problem arises due to an internal data store call made by any ORM framework to perform a single operation. The problem could be outlined as follows:

+   1. A list of entities was returned that will be further iterated to perform some additional operation.
+   2. Assuming a navigational property is accessed inside the iteration:
+      - Each time an iteration tries to access the navigational property, a database hit occurs
+      - The hits will happen N times for N number of items returned in the first step
+   3. This makes the application perform `N+1 Select` (N for the iterations and 1 for the first call that retrieves the list), which should have been performed in a single database call.

This behavior is often termed as **lazy loading** in any ORM frameworks, which loads the subsequent/dependent data only when required. The lazy loading is yet to be supported by EF Core. This can be verified in the EF Core road map, which lists it as **Stretch goals**, as shown in the following screenshot:

![](bbox=[0.21, 0.603, 0.793, 0.858])