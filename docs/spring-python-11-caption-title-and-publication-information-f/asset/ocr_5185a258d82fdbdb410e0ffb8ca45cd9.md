Let's restart it and call the client script again.

![](bbox=[0.222, 0.172, 0.777, 0.32])

This shows that our server code is being called from the client. And it's nicely decoupled from the machinery of Pyro.

# Is our example contrived?

Does this example appear contrived? Sure it does. However, the basic concept of accessing a service from a client is not.

Instead of our Happy Birthday service, this could be the data access layer of an application, an interface into an airline flight reservation system, or access to trouble ticket data offered by an operations center.

Our example still has the same fundamental concepts of input arguments, output results, and client-side processing after the fact.

# Spring Python is non-invasive

We took a very simple service, and by serving it up through an IoC container, it was easy to wrap it with a Pyro exporter without our application realizing it. The IoC container, as demonstrated throughout this book, opens the door to many options. Now, we see how it lends itself to exposing our code as a Pyro service. This concept doesn't stop with Pyro. This same pattern can be applied to export services for other remoting mechanisms. By being able to separate the configuration from the actual business logic, we can yet again apply a useful and practical service without having to rewrite the code to work with the service.