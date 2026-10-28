Consider the example of calling two isolated APIs that have no dependencies on each other. The sequence diagram for this may look something like the following:

![](bbox=[0.1, 0.22, 0.92, 0.66])

This linear process of calling A, and then calling B when A is done, is simple, as it requires no complex orchestration to wrangle the results of the API calls, but it is slower than necessary. By calling both APIs synchronously and in sequence, we waste time waiting for them to return. A better way of doing this, if we don't require the result of the first API for the request to the second, may be to call them together asynchronously.