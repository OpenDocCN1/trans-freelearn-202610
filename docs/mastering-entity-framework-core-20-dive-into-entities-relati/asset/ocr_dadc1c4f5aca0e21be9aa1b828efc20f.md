Trying to access anyone of the preceding link would throw the following error:

![](bbox=[0.247, 0.153, 0.753, 0.347])

For the sake of argument, let's change the parameter type of the identifier to a string and try injecting the value again to the `Details` action to see how SQL injection is really handled by parameterized queries. It is evident in the following screenshot that the parameterized query is trying to convert the value to an integer that actually fails and the execution is stopped, thereby protecting the application against SQL injection:

![](bbox=[0.105, 0.481, 0.895, 0.876])