# 2

# Failing to Understand the Role of DDD

It could be argued that an anti-pattern when building microservices is not having a deep understanding of the business domain or the problem space and then trying to implement microservice tactical patterns. This can lead to chaos or a large, distributed monolith that is tightly coupled and overly complex.

Microservices are collections of services that are designed to meet a common goal or serve a particular business feature. Microservices must work together to meet this common goal. In some cases, a service may be completely unaware of its role when it comes to the bigger picture or may not have knowledge of any other microservices. On the other hand, the teams that build these services should understand the bigger picture and how each service helps achieve the greater goal of the system as a whole or where their service fits in the composition of a larger application.