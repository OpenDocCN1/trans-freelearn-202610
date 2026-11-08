In the preceding example, we first create an Observable that will propagate three numbers (1, 2, and 3). Then, we apply the `filter()` operator over this Observable; this will create a new Observable that will emit only the value i, where i %2 === 1. So the new Observable propagates only odd numbers.

If you run the preceding code, you will see this output:

1
3

This operation can be represented by the following diagram as well:

![](bbox=[0.17, 0.34, 0.75, 0.65])

In this example, we are always returning true or false, but we can also return any truthy or falsy value.

![](bbox=[0.06, 0.71, 0.14, 0.79])

A truthy value is any object that acts true when evaluated as a Boolean. Falsy values are those that act false when evaluated as a Boolean.