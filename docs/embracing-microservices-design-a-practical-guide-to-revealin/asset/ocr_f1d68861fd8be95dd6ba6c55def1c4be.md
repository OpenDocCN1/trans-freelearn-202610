In the preceding diagram, when the first request was made, the circuit was closed. When the service responded with an HTTP 500 error, it opened the circuit for a specific amount of time. When the circuit was open, none of the requests were sent to a target service. Once the time had elapsed, the circuit became half open and allowed the first request to be sent out. Finally, when the service responded with an HTTP 200 status code, the circuit was closed once more.

You can also use the circuit breaker pattern in conjunction with the retry pattern so that when the circuit is closed and the request fails, it will not open the circuit until the retry counter reaches its maximum retry limit. The following diagram shows using the retry pattern in conjunction with the circuit breaker pattern:

![](bbox=[0.1, 0.36, 0.85, 0.65])

Figure 4.10 – Using the retry pattern in conjunction with the circuit breaker pattern

Various libraries allow you to easily implement the retry and circuit breaker patterns. The following table shows the most common ones that are used for .NET and Java applications:

| Library | Platform | More Information |
|---|---|---|
| Polly | .NET applications | https://github.com/App-vNext/Polly |
| Netflix Hystrix | Java applications | https://github.com/Netflix/Hystrix |

Let us next see how important scalability is when designing a microservice.